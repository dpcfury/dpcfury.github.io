---
title: Leetcode[355] Design Twitter
date: 2020-04-13 23:43:06
urlname: design-twitter
categories:
- leetcode
tags:
- leetcode
- heap
- design
---
>[Leetcode 355. Design Twitter](https://leetcode.com/problems/design-twitter/)
题目要求设计一个简要的Twitter系统，具有基本的几个功能，其实感觉设计题就是着重在如何利用数据结构完成业务，只要做好规划，写好设计框架，再慢慢实现和调试，基本都能cover住。

<!--more-->

#### 思路
- 抽象出主要的数据结构（User，Post）
- 定义好对象的主要动作
- 抽象出主要业务的逻辑模型，分步实现
- 注意细节，单词瓶拼写，特比是followerId，followeeId 非常相似（坑了半小时不止）
- 时间戳因为计算机性能很强，容易没有diff，需要一个全局一致的间隔量来实现更合适

#### 算法实现
```java
class Twitter {

    public static int interval = 0;

    static class Post {
        int id;
        long timeStamp;

        public Post(int postId) {
            this.id = postId;
            this.timeStamp = System.currentTimeMillis() + Twitter.interval++;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Post post = (Post) o;
            return id == post.id;
        }

        @Override
        public int hashCode() {
            return Objects.hash(id, timeStamp);
        }
    }

    static class User {
        int id;
        LinkedList<Post> posts;
        Map<Integer, Post> index;
        Set<Integer> followers;

        private List<Integer> getFollowers() {
            List<Integer> res = new ArrayList<>(followers);
            if (!followers.contains(this.id)) {
                res.add(this.id);
            }
            return res;
        }

        private User(int userId) {
            this.id = userId;
            posts = new LinkedList<>();
            followers = new HashSet<>();
            index = new HashMap<>();
            followers.add(userId);
        }

        private void psotTweet(int tweetId) {
            if (!index.containsKey(tweetId)) {
                Post post = new Post(tweetId);
                posts.addFirst(post);
                index.put(tweetId, post);
            }
        }

        // 获取自己的最近10篇tweet
        private List<Post> getNeesFeed() {
            List<Post> res = new ArrayList<>(10);
            for (int i = 0; i < posts.size() && i < 10; i++) {
                res.add(posts.get(i));
            }
            return res;
        }

        private void follow(int followeeId) {
            followers.add(followeeId);
        }

        private void unfollow(int followeeId) {
            followers.remove(followeeId);
        }

        @Override
        public String toString() {
            return "User{" +
                    "id=" + id +
                    ", followers=" + followers +
                    '}';
        }
    }

    private HashMap<Integer, User> users;

    /**
     * Initialize your data structure here.
     */
    public Twitter() {
        this.users = new HashMap<>();
    }

    /**
     * Compose a new tweet.
     */
    public void postTweet(int userId, int tweetId) {
        if (!users.containsKey(userId)) {
            // 初始化用户，并创建博客
            User user = new User(userId);
            users.put(userId, user);
        }
        users.get(userId).psotTweet(tweetId);
    }

    /**
     * Retrieve the 10 most recent tweet ids in the user's news feed.
     * Each item in the news feed must be posted by users who the user followed or by the user herself.
     * Tweets must be ordered from most recent to least recent.
     */
    public List<Integer> getNewsFeed(int userId) {
        if (!users.containsKey(userId)) return new ArrayList<>();
        else {
            List<Integer> res = new ArrayList<>();
            PriorityQueue<Post> tempRes = new PriorityQueue<>(new Comparator<Post>() {
                @Override
                public int compare(Post o1, Post o2) {
//                    System.out.println(o1.timeStamp);
//                    System.out.println(o2.timeStamp);
                    int diff = (int) (o1.timeStamp - o2.timeStamp);
//                    System.out.println("diff" + diff);
                    return diff;
                }
            });
            User user = users.get(userId);
            List<Post> posts = new ArrayList<>();
            for (int followeeId : user.getFollowers()) {
                User followee = users.get(followeeId);
                posts.addAll(followee.getNeesFeed());
            }
            for (Post post : posts) {
                if (tempRes.size() != 10) tempRes.offer(post);
                else {
                    Post min = tempRes.peek();
                    if (min.timeStamp < post.timeStamp) {
                        tempRes.poll();
                        tempRes.offer(post);
                    }
                }
            }
            while (!tempRes.isEmpty()) {
                Post post = tempRes.poll();
                res.add(post.id);
            }
            Collections.reverse(res);
            return res;
        }
    }

    /**
     * Follower follows a followee. If the operation is invalid, it should be a no-op.
     */
    public void follow(int followerId, int followeeId) {
        if (!users.containsKey(followerId)) {
            // 初始化用户，并创建博客
            User user = new User(followerId);
            users.put(followerId, user);
        }

        if (!users.containsKey(followeeId)) {
            // 初始化用户，并创建博客
            User user = new User(followeeId);
            users.put(followeeId, user);
        }

        User follower = users.get(followerId);

        follower.follow(followeeId);
    }

    /**
     * Follower unfollows a followee. If the operation is invalid, it should be a no-op.
     */
    public void unfollow(int followerId, int followeeId) {
        if (users.containsKey(followerId) && users.containsKey(followeeId)) {
            User user = users.get(followerId);
            user.unfollow(followeeId);
        }
    }

    public static void main(String[] args) {
//

        Twitter twitter = new Twitter();
        twitter.postTweet(1, 1);
        twitter.unfollow(2, 2);

    }

}
```
