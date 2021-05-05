# CVE-2021-21349 XStream SSRF

## XStream Official Announcement

[CVE-2021-21349](https://x-stream.github.io/CVE-2021-21349.html)

**Vulnerability**

CVE-2021-21349: A Server-Side Forgery Request can be activated unmarshalling with XStream to access data streams from an arbitrary URL referencing a resource in an intranet or the local host.

**Affected Versions**

All versions until and including version 1.4.15 are affected, if using the version out of the box. No user is affected, who followed the recommendation to setup XStream's security framework with a whitelist limited to the minimal required types.

**Description**

The processed stream at unmarshalling time contains type information to recreate the formerly written objects. XStream creates therefore new instances based on these type information. An attacker can manipulate the processed input stream and replace or inject objects, that result in a server-side forgery request.

## Docker Demo

![cve-2021-21349](https://user-images.githubusercontent.com/56715563/117110850-b6acbf80-adc1-11eb-9e4b-80e9389290c1.gif)

## Set Up XStream Environment & PoC Execution

1. Build an image from a Dockerfile (Set Up)

```
$ docker build -t cve-2021-21349 .
```

2. Run Http Server

```
$ python -m http.server 8080
```

3. Run java -jar xstream in a new container (PoC Execution)

```
$ docker run -it --rm cve-2021-21349
```

## Output

```
Serving HTTP on :: port 8080 (http://[::]:8080/) ...
::ffff:127.0.0.1 - - [05/May/2021 16:35:39] "GET /internal.txt HTTP/1.1" 200 -
```

## Solution

- Update xstream version to 1.4.16 or higher

Change pom.xml to bellow

```
        <dependency>
            <groupId>com.thoughtworks.xstream</groupId>
            <artifactId>xstream</artifactId>
            <version>1.4.16</version>
        </dependency>
```

- Use XStream's security framework

Add NoTypePermission.NONE

```
import com.thoughtworks.xstream.security.NoTypePermission; // Add

XStream xstream = new XStream();
xstream.addPermission(NoTypePermission.NONE); // Add
xstream.fromXML(xml);
```
