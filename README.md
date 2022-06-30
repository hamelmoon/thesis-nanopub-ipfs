# Connecting the IPFS and Nanopublication network to enhance the accessibility of Linked Open Data: A Proof-of-Concept Study

## Server: Nanopub server
https://github.com/hamelmoon/nanopub-server

### Setting up the development environment of Nanopub-server 
#### Prerequisite

Maven

Docker

Java 1.8 SDK

InteliJ

#### Clone source code

`git clone https://github.com/hamelmoon/nanopub-server.git`

This is a forked repo.

#### Run Mongo container with no authentication

`docker run -d  --name mongo-on-docker  -p 27888:27017 mongo`

#### Run IPFS node container

`docker run -d --name ipfs_local_node -p 4001:4001 -p 4001:4001/udp -p 127.0.0.1:5001:5001 ipfs/go-ipfs:latest`

#### Edit Local properties

```
$ cd nanopub-server/src/main/resources/ch/tkuhn/nanopub/server/
$ cp conf.properties local.conf.properties
```

#### Run (Command line)

`mvn jetty:run`

#### Hot swapping with maven, jetty setting up

No more server restarts. The trick to getting hot swapping to work is to attach a remote debugger to your Jetty process.

****Create a Maven run configuration for Jetty in IntelliJ****

1. From IntelliJ, click `Run` > `Edit Configurations`
2. Click `Add New Configuration` (the plus sign)
3. Choose `Maven`
4. Name it `Nanopub-jetty` (or whatever you like)
5. Choose the appropriate working directory
6. In Goals, enter `jetty:run`
7. Click the `Runner` tab
8. In VM Parameters, enter `Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=4000`. Note: the address here corresponds to the port you will debug with. Using 4000 leaves the more standard 5005 debug port open for other debuggers.
9. Click `Ok`

****jetty-maven-plugin****

ensure scanIntervalSeconds is set to 0

```jsx
<plugin>
		<groupId>org.eclipse.jetty</groupId>
    <artifactId>jetty-maven-plugin</artifactId>
    ...
    <configuration>
        <scanIntervalSeconds>0</scanIntervalSeconds>
        ...
    </configuration>
</plugin>
```

#### Debug env setting up

****Setup Reloading Classes After Compilation****

1. From IntelliJ, open `Settings` (the wrench icon)
2. Navigate to `Debugger` > `HotSwap`
3. Choose `Always` for `Reload classes after compilation`
4. Click `Ok`

****Create an Application debug configuration****

1. From IntelliJ, click `Run` > `Edit Configurations`
2. Click `Add New Configuration` (the plus sign)
3. Choose `Remote`
4. Name it `Nanopub-jetty-hotswap` (or whatever you like)
5. In the `Port` field, enter `4000`. Note: If you modified the address in step 1.8 above, the port should match.
6. Click `Ok`

****Debug run****

1. Run the run target, `Nanopub-jetty`
2. Run the debug target, `Nanopub-jetty-hotswap`

#### Reference url

[https://gist.github.com/naaman/1053217/7f7cf7d9d373a8c56731033f9becfb7c9533c917](https://gist.github.com/naaman/1053217/7f7cf7d9d373a8c56731033f9becfb7c9533c917)

[https://wiki.eclipse.org/Jetty/Feature/Jetty_Maven_Plugin](https://wiki.eclipse.org/Jetty/Feature/Jetty_Maven_Plugin)

[https://docs.ipfs.io/how-to/run-ipfs-inside-docker/#set-up](https://docs.ipfs.io/how-to/run-ipfs-inside-docker/#set-up)

• Java library: [nanopub-java](https://github.com/Nanopublication/nanopub-java) ([paper](https://arxiv.org/abs/1508.04977))

• Decentralized nanopublication server network: [monitor](http://purl.org/nanopub/monitor) / [example server](http://server.nanopubs.lod.labs.vu.nl/) / [paper 1](http://arxiv.org/pdf/1411.2749) / [paper 2](https://doi.org/10.7717/peerj-cs.78) ([server code](https://github.com/tkuhn/nanopub-server) / [monitor code](https://github.com/tkuhn/nanopub-monitor/))

## Evaluation: Performance Test Setup
https://github.com/hamelmoon/nanopub-ngrinder-setup

## Monitoring: Prometheus, Grafana Setup
https://github.com/hamelmoon/Docker-Compose-Prometheus-and-Grafana

## References
* nanopub-monitor : https://github.com/tkuhn/nanopub-monitor
* nanopub-server : https://github.com/tkuhn/nanopub-server
* nanopub-java : https://github.com/Nanopublication/nanopub-java
* peergos-hamt : https://github.com/Peergos/Peergos/blob/master/src/peergos/shared/hamt/Champ.java
* IPLD HAMT Advanced Data Layout :  https://ipld.io/specs/advanced-data-layouts/hamt/
* IPFS docs: https://docs.ipfs.io/reference/http/api
* Guice(Dependency Injection) : https://github.com/google/guice
