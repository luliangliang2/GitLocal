## log4j.properties

#### 配置log4j

```properties
log4j.rootLogger=debug, PubLog

log4j.logger.gemini.logs.utils.logts=error,PubLog
log4j.appender.PubLog=org.apache.log4j.RollingFileAppender
log4j.appender.PubLog.layout=org.apache.log4j.PatternLayout
log4j.appender.PubLog.layout.ConversionPattern=%m %x %n
log4j.appender.PubLog.Append = true
log4j.appender.PubLog.Threshold = error
log4j.appender.PubLog.MaxFileSize = 100M
log4j.appender.PubLog.MaxBackupIndex = 1000
log4j.appender.PubLog.File=/Users/lll/Downloads/log/PubLog.log
```


### 日志输出格式

%n- 换行  
%m - 日志内容  
%p - 日志级别(FATAL, ERROR, WARN, INFO, DEBUG or custom)  
%r - 程序启动到现在的毫秒数  
%% - percent sign in output  
%t - 当前线程名  
%d - 日期和时间，常用的格式有 %d{DATE}, %d{ABSOLUTE},   %d{HH:mm:ss,SSS},  
%F - java源文件名  
%L - java源码行数  
%C - java类名,%C{1} 输出最后一个元素  
%M-java方法名  
%l - 同 %F%L%C%M  
 ### 日志输出的等级

* 优先级从高到低依次为:
OFF >   
FATAL>   
ERROR>   
WARN >  
INFO>  
DEBUG >  
TRACE >  
ALL
*
*  ALL    最低等级的 用于打开所有日志记录
*  TRACE  很低的日志级别 一般不会使用
*  DEBUG  指出细粒度信息事件对调试应用程序是非常有帮助的 主要用于开发过程中打印一些运行信息
*  INFO   消息在粗粒度级别上突出强调应用程序的运行过程
*        打印一些你感兴趣的或者重要的信息 这个可以用于生产环境中输出程序运行的一些重要信息
*        但是不能滥用 避免打印过多的日志
*  WARN   表明会出现潜在错误的情形 有些信息不是错误信息 但是也要给程序员的一些提示
*  ERROR  指出虽然发生错误事件 但仍然不影响系统的继续运行
*        打印错误和异常信息 如果不想输出太多的日志 可以使用这个级别
*  FATAL  指出每个严重的错误事件将会导致应用程序的退出
*        这个级别比较高了 重大错误 这种级别你可以直接停止程序了
*  OFF    最高等级的，用于关闭所有日志记录
*
*  如果将log level设置在某一个级别上 那么比此级别优先级高的log都能打印出来
*  例如 如果设置优先级为WARN 那么OFF FATAL ERROR WARN 4个级别的log能正常输出
*  而INFO DEBUG TRACE ALL级别的log则会被忽略
