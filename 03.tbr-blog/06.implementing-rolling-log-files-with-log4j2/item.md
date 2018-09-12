---
title: 'Implementing Rolling Log Files with log4j2'
date: '14:06 12-09-2018'
---

It is necessary to keep track of log traces, without having too big files (matter of transportation, if you communicate with a customer), it is necessary to configure a rolling appender in the logging framework.
Here is an example with log4j2

````
        <RollingRandomAccessFile name="logFile" fileName="/logs/appname/test/test.log" filePattern="appname-%d{MM-dd-yyyy}.log.gz"  append="true">
            <PatternLayout>
                <Pattern>${patternLayout}</Pattern>
            </PatternLayout>
            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="20 MB"/>
            </Policies>
            <DefaultRolloverStrategy max="200"/>
        </RollingRandomAccessFile>
````
