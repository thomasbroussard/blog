---
title: 'Using the maven-shade-plugin'
date: '13:52 12-09-2018'
---

When it is needed to build a "fat" jar (a jar that contains all the dependencies tree), it is possible to do it with the maven shade plugin:

```
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-shade-plugin</artifactId>
				<version>3.1.1</version>
				<executions>
					<execution>
						<phase>package</phase>
						<goals>
							<goal>shade</goal>
						</goals>
						<configuration>
							<filters>
								<filter>
									<artifact>*:*</artifact>
									<excludes>
										<exclude>META-INF/*.SF</exclude>
										<exclude>META-INF/*.DSA</exclude>
										<exclude>META-INF/*.RSA</exclude>
									</excludes>
								</filter>
							</filters>
							<transformers>
								<transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
									<resource>META-INF/spring.handlers</resource>
								</transformer>
								<transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
									<resource>META-INF/spring.schemas</resource>
								</transformer>
								<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
									<mainClass>com.your.package.YourClass</mainClass>
								</transformer>
							</transformers>
						</configuration>
					</execution>
				</executions>
			</plugin>
```

