With this plugin in you can restrict access from any jar/war/ear dependency to target package/class.

We use this plugin to be ensure that some restricted package are not accessed by in-house developed components.

We generally restrict access to reflection, threads or some architectural components.

You can use wildcard both in from and to targets with exceptions.

Following is a sample usage from integration test, please note that restrictions are meaningless; just for testing.

{code:xml}
  <build>
        <plugins>
            <plugin>

                <groupId>com.yamanyar</groupId>
                <artifactId>restrict-maven-plugin</artifactId>
                <version>0.1-SNAPSHOT</version>

                <executions>
                    <execution>
                        <phase>verify</phase>
                        <goals>
                            <goal>restrict</goal>
                        </goals>
                    </execution>
                </executions>

                <!-- following config is for demonstration only. It is not really meaningful to restrict access to java.util.regex.* :). -->
                <configuration>
                    <!-- If you set to false; build will be broken; otherwise although there are access violation; build will be successful. -->
                    <continueOnError>false</continueOnError>
                    <restrictions>
                        <!-- Restrict all access from com.ya* (except from com.yamanyar.test.MyTestDef) to  java.util.regex.*-->
                        <restriction>com.ya*,!com.yamanyar.test.MyTestDef -> java.util.regex.*</restriction>
                        <!-- Restrict all access from * (except from *MyTestDef and com.yamanyar.none.*) to  sun.net.www.http.HttpClient -->
                        <restriction>*,!*MyTestDef,!com.yamanyar.none.* -> sun.net.www.http.HttpClient</restriction>
                        <!-- Restrict all access from org.apache.commons.io.* to java.nio.* (except to java.nio.Buffer) -->
                        <restriction>org.apache.commons.io.* -> java.nio.*,!java.nio.Buffer</restriction>
                        <!-- This is not tested in integration test; just for sample usage
                        Restrict all access from org.*,com.* (except from net.*,gov.*)
                                to co.uk.*,com.tr.* (except to eu.*,li.*)-->
                        <restriction>org.*,com.*,!net.*,!gov.* -> co.uk.*,com.tr.*,!eu.*,!li.*</restriction>
                    </restrictions>
                </configuration>

            </plugin>
        </plugins>
    </build>

 {code}

Here is a more meaningful fragment from our company:
{code:xml}
    <restrictions>
        <restriction>com.ykb.* -> java.lang.Thread,java.net.*,java.io.*,java.sql.*,java.lang.reflect.*,javax.transaction.*,java.util.concurrent.*,com.ibm.*,!java.io.Serializable</restriction>
    </restrictions>
{code}
