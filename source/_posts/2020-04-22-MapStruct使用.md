---
title: MapStruct使用
date: 2020-04-22 22:53:27
urlname: mapstruct-intro
categories:
- 开发
tags:
- spring-boot
- Java
---
>[MapStruct](https://mapstruct.org/)
在当前的系统设计中，经常会在PO、VO，DTO之间来回转换，为了在业务代码中尽量不让set、get打断代码逻辑，方便阅读理解代码，需要提供对象之间灵活转换的解决方案，而MapStruct正是今天介绍的一种映射库。只需要定义一个 Mapper 接口，MapStruct 就会自动实现这个映射接口，避免了复杂繁琐的映射实现。

<!--more-->

#### 依赖引入
```bash
<dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct</artifactId>
            <version>1.3.1.Final</version>
        </dependency>
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct-processor</artifactId>
            <version>1.3.1.Final</version>
        </dependency>
```
>ps: springboot没有提供默认的mapstruct版本，所以在boot中引用需要手动指定对应的版本号，目前的稳定版本是** 1.3.1.Final**

#### 接口编写

假设我们需要转换Host（机器实体）到HostDTO（rpc、http传输对象），只需要编写下面的接口
```java
@Mapper(componentModel = "spring") // 利用spring注入
public interface HostMapper {

    @Mappings({
            @Mapping(source= "sn", target = "sn"),
            @Mapping(source= "hostName", target = "hostName"),
            @Mapping(source= "iloMac", target = "iloMac"),
            @Mapping(source= "mac1", target = "mac1"),
            @Mapping(source= "cpu", target = "cpu"),
            @Mapping(source= "memory", target = "memory"),
            @Mapping(source= "hardDisk", target = "hardDisk"),
            @Mapping(source= "ssd", target = "ssd"),
            @Mapping(source= "model", target = "model"),
            @Mapping(source= "manufacturer", target = "manufacturer"),
            @Mapping(source= "os", target = "os"),
            @Mapping(source= "description", target = "description"),
            @Mapping(source= "arrivedTime", target = "arrivedTime"),
            @Mapping(source= "extraInfo", target = "extraInfo"),
            @Mapping(source= "suit", target = "suit")
    })
    public Host from(HostDTO hostDTO);


    @Mappings({
            @Mapping(source= "sn", target = "sn"),
            @Mapping(source= "hostName", target = "hostName"),
            @Mapping(source= "iloMac", target = "iloMac"),
            @Mapping(source= "mac1", target = "mac1"),
            @Mapping(source= "cpu", target = "cpu"),
            @Mapping(source= "memory", target = "memory"),
            @Mapping(source= "hardDisk", target = "hardDisk"),
            @Mapping(source= "ssd", target = "ssd"),
            @Mapping(source= "model", target = "model"),
            @Mapping(source= "manufacturer", target = "manufacturer"),
            @Mapping(source= "os", target = "os"),
            @Mapping(source= "description", target = "description"),
            @Mapping(source= "arrivedTime", target = "arrivedTime"),
            @Mapping(source= "extraInfo", target = "extraInfo"),
            @Mapping(source= "suit", target = "suit")
    })
    public HostDTO from(Host host);


}
```

#### 测试
我们在Junit中尝试使用这个mapper进行对象的转换：
```java
@SpringBootTest
public class TestMapper {

    @Resource
    private HostMapper hostMapper; // IOC 依赖注入

    @Test
    public void testHostMapper() {
        HostDTO hostDTO = new HostDTO();
        hostDTO.setSn("host01");
        hostDTO.setHostName("host01.maas");
        hostDTO.setIloMac("weqweqweqw");
        hostDTO.setMac1("sdasdasdsa");
        hostDTO.setCpu("core 24");
        hostDTO.setMemory("512GB");
        hostDTO.setHardDisk("8600GB");
        hostDTO.setSsd("6x480");
        hostDTO.setModel("xxxx");
        hostDTO.setManufacturer("H3C");
        hostDTO.setArrivedTime(new Date(System.currentTimeMillis()));
        hostDTO.setSuit("Y32-X14");

        Host host = hostMapper.from(hostDTO);

        System.out.println(host);

    }
}
```