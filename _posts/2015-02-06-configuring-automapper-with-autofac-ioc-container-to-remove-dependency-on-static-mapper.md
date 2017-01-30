---
layout: post
title: "Configuring Automapper with Autofac IOC Container to remove dependency on static Mapper"
date: 2015-02-06 -0800
comments: true
tags: [autofac, automapper, automapper with autofac]
redirect_from:
  - index.php/configuring-automapper-with-autofac-ioc-container-to-remove-dependency-on-static-mapper/
  - index.php/tag/autofac/
  - index.php/tag/automapper/
  - index.php/tag/automapper-with-autofac/
---
Automapper currently provides a static class Mapper to do different operations, Mapper is just another wrapper on IMappingEngine, IConfiguration and IConfigurationProvider. So by dependency injecting these interfaces along with few others the Mapper static class can be avoided. This can be done by any of the IOC container, [Jimmy](https://lostechies.com/jimmybogard/2009/05/12/automapper-and-ioc/) has written a nice post on how to do it in StructureMap, the code below shows how to do it in Autofac

```csharp
builder.RegisterType<TypeMapFactory>().As<ITypeMapFactory>();
builder.RegisterType<ConfigurationStore>()
       .As<ConfigurationStore>()
       .WithParameter("mappers", MapperRegistry.Mappers)
       .SingleInstance();
 
builder.Register((ctx, t) => ctx.Resolve<ConfigurationStore>())
       .As<IConfiguration>()
       .As<IConfigurationProvider>();
 
builder.RegisterType<MappingEngine>().As<IMappingEngine>();
```

Note that ConfigurationStore is registered as singleton as there will be only one configuration object in single app domain. MappingEngine can be registered as singleton also but it does not really matter as it is fairly lightweight class.

The configuration can be unit test using following unit test

```csharp
var container = GetAutofacContainer();
const int expectedValue = 15;

var configuration1 = container.Resolve<IConfiguration>();
var configuration2 = container.Resolve<IConfiguration>();
Assert.AreSame(configuration1,configuration2);

var configurationProvider = container.Resolve<IConfigurationProvider>();
Assert.AreSame(configurationProvider,configuration1);

var configuration = container.Resolve<ConfigurationStore>();
Assert.AreSame(configuration,configuration1);

configuration1.CreateMap<Source, Destination>();

var engine = container.Resolve<IMappingEngine>();

var destination = engine.Map<Source, Destination>(new Source { Value = expectedValue });

Assert.AreEqual(expectedValue,destination.Value);
```

Thats all we need to remove the dependency on Mapper static class using Autofac