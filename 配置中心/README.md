# 配置管理

配置是软件开发，部署，运维全生命周期的重要组成，传统的配置方式包括了硬编码，配置文件，数据库配置表等。

# 硬编码

硬编码是最基础的配置方案，其简单易用：

```java
public class AppConfig {

    private int connectTimeoutInMills = 5000;

    public int getConnectTimeoutInMills() {
        return connectTimeoutInMills;
    }

    public void setConnectTimeoutInMills(int connectTimeoutInMills) {
        this.connectTimeoutInMills = connectTimeoutInMills;
    }
}
```

这种形式主要有三个问题：

- 难以动态修改，如果配置是需要动态修改的话，需要当前应用去暴露管理该配置项的接口，至于是 Controller 的 API 接口，还是 JMX ，都是可以做到。

- 难以持久化与恢复，配置变更都是发生在内存中，并没有持久化。因此，在修改配置之后重启应用，配置又会变回代码中的默认值了。

- 难以分布式多机器同步，当你有多台机器的时候，要修改一个配置，每一台都得去操作一遍，运维成本可想而知。

# 配置文件

Spring 中常见的 properties、yml 文件，或其他自定义的，如，“conf”后缀等：

```yaml
# application.properties
connectTimeoutInMills=5000
```

相比“硬编码”的形式，它解决了第二个问题，持久化了配置。但是，另外两个问题并没有解决，运维成本依旧还是很高的。

配置动态变更，可以是通过类似“硬编码”暴露管理接口的方式，这时，代码中会多一步持久化新配置到文件的逻辑。或者，简单粗暴点，直接登录机器上去修改配置文件，再重启应用，让配置生效。当然，你也可以在代码中增加一个定时任务，如每隔 10s 读取配置文件内容，让最新的配置能够及时在应用中生效，这样也就免去了重启应用这个“较重”的运维操作。

# DB 配置表

这里的 DB 可以是 MySQL 等的关系型数据库，也可以是 Redis 等的非关系型数据库。数据表如：

```sql
CREATE TABLE `config` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `key` varchar(50) NOT NULL DEFAULT '' COMMENT '配置项',
  `value` varchar(50) NOT NULL DEFAULT '' COMMENT '配置内容',
  `updated_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `created_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_key` (`key`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='配置信息';

INSERT INTO `config` (`key`, `value`, `updated_time`, `created_time`) VALUES ('connectTimeoutInMills', '5000', CURRENT_TIMESTAMP, CURRENT_TIMESTAMP);
```

它相对于前两者，更进一步，将配置从应用中抽离出来，集中管理，能较大的降低运维成本。

那么，它能怎么解决动态更新配置的问题呢？据我所知，有两种方式。

其一，如同之前一样，通过暴露管理接口去解决，当然，也一样得增加持久化的逻辑，只不过，之前是写文件，现在是将最新配置写入数据库。不过，程序中还需要有定时从数据库读取最新配置的任务，这样，才能做到只需调用其中一台机器的管理配置接口，就能把最新的配置下发到整个应用集群所有的机器上，真正达到降低运维成本的目的。

其二，直接修改数据库，程序中通过定时任务从数据库读取最新的配置内容。

“DB 配置表”的形式解决了主要的问题，但是它不够优雅，带来了一些“累赘”。

# 链接

- https://mp.weixin.qq.com/s/3JIzG7HhIgL1dGzYMS37pw
