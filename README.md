### Cipher encrypt library

You can use this library to encrypt and decrypt messages.

### Usage:

## How to use?

Add the following library dependency in your `build.sbt` file. This will import all the AWS service modules in your project.
##### sbt
```
resolvers += "google-artifact-registry" at "https://asia-maven.pkg.dev/sonarqube-289802/knoldus-aws-lib/"

libraryDependencies += "knoldus" % "cipher_lib_2.13" % "1.0"
```

##### Maven
```<dependency>
    <groupId>knoldus</groupId>
    <artifactId>cipher_lib_2.13</artifactId>
    <version>1.0</version>
</dependency>
```

##### Gradle
```
compile group: 'knoldus', name: 'cipher_lib_2.13', version: '1.0'
```


##### Creating com.knoldus.cipher.CipherService object 
```
    val client = new com.knoldus.client.InternalClient(new com.knoldus.client.FakeCiphers())
    val cipher = new com.knoldus.cipher.CipherService(client)
```
##### Use method .create to encrypt text message. Type String
```
    val encryptedCipher = cipher.create("Test string for encrypt", Map.empty)
```
##### This method will return model com.knoldus.models.EncryptedCipher
```
    case class com.knoldus.models.EncryptedCipher(
        id: String,
        password: String
    )
```    
##### Use method .get to decrypt. Type com.knoldus.models.EncryptedCipher
```
    decryptedCipher = cipher.get(encryptedCipher)
```
##### This method will return model com.knoldus.models.EncryptedCipher
```
    case class com.knoldus.models.DecryptedCipher(
        id: String,
        cleartext: String,
        attributes: Map[String, String]
    )
```

### Example :
```
id - icc3AINJrw1GsGyDUioMwLcOEQriZZnMkw1S9DQI2BgxQmyTjuV9SRSm0HZgrJso : password - DaIptksNZuIWNMPsmAMMnxjIWYzfASR8rtD69B6nbtjziHATF5Qy7RRhc3IhP2R8
id - icc3AINJrw1GsGyDUioMwLcOEQriZZnMkw1S9DQI2BgxQmyTjuV9SRSm0HZgrJso : cleartext - Test string for encrypt
```


##### Sample Code 
```
  implicit lazy val system: ActorSystem = ActorSystem()
  implicit private val ec: ExecutionContext = system.dispatcher

  val client = new InternalClient(new FakeCiphers())

  val cipher = new CipherService(client)

  val encryptedCipher: Future[Either[String, EncryptedCipher]] = cipher.create("Test string for encrypt", Map.empty)

  encryptedCipher.map(x =>
    x.map { y =>
      println(s"id - ${y.id} : password - ${y.password}")
      val decryptedCipher = cipher.get(y)
      decryptedCipher.map(x => x.map(y => println(s"id - ${y.id} : cleartext - ${y.cleartext} ")))
    }
  )
```

#### Sample POM.XML
```.env
<?xml version='1.0' encoding='UTF-8'?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>
    <groupId>knoldus</groupId>
    <artifactId>cipher_lib_2.13</artifactId>
    <packaging>jar</packaging>
    <description>cipher_lib</description>
    <version>1.0</version>
    <name>cipher_lib</name>
    <organization>
        <name>knoldus</name>
    </organization>
    <dependencies>
        <dependency>
            <groupId>org.scala-lang</groupId>
            <artifactId>scala-library</artifactId>
            <version>2.13.10</version>
        </dependency>
        <dependency>
            <groupId>com.github.sps.junidecode</groupId>
            <artifactId>junidecode</artifactId>
            <version>0.3</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.12.0</version>
        </dependency>
        <dependency>
            <groupId>org.scalatestplus.play</groupId>
            <artifactId>scalatestplus-play_2.13</artifactId>
            <version>5.1.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <repositories>
        <repository>
            <id>googleartifactregistry</id>
            <name>google-artifact-registry</name>
            <url>https://asia-maven.pkg.dev/sonarqube-289802/knoldus-aws-lib/</url>
            <layout>default</layout>
        </repository>
    </repositories>
</project>
```
