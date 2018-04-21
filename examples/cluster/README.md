# Akkatecture Cluster Sample

The Cluster Sample is made up of three projects. The descriptions of the projects will come acompanied by the overall sample description.

## [ClusterClient](https://github.com/Lutando/Akkatecture/tree/master/examples/cluster/Akkatecture.Examples.ClusterClient)

The cluster client project is the code that interfaces with the cluster. It can be seen as the way to invoke the domain from an external place. It works by opening a proxy to the actual worker(s). All requests done through the client are transported/serialized/deserialized through the actor system and the requests are routed to the correct cluster shard that runs on the worker.

## [Worker](https://github.com/Lutando/Akkatecture/tree/master/examples/cluster/Akkatecture.Examples.Worker)

The worker is where all the real work is done. The worker is responsible for routing commands to the right aggregates and the aggregates then process the commands, which may or may not result in events being emitted from them, which will result in events being persisted to their event journal. In typical scenarios the worker would be deployed in numerous nodes within a network on the cloud or in a kubernetes cluster.

## [Seed](https://github.com/Lutando/Akkatecture/tree/master/examples/cluster/Akkatecture.Examples.Seed)

The seed or seed node is only responsible for akka service discovery. Its only purpose is to exist at a well known location so that nodes within the actor system can effectively discover each other through the seed node. It is important that the seed does no work that can potentially disrupt its service level. In this sample the seeds address is [127.0.0.1:6000](https://github.com/Lutando/Akkatecture/blob/6dc8653e324931e61097a8f2dd48916706b16743/examples/cluster/Akkatecture.Examples.Seed/seed.conf#L6).

### Description

This sample uses the same domain model as the one found in the [simple](https://github.com/Lutando/Akkatecture/tree/master/examples/simple/Akkatecture.Examples.UserAccount) sample.

If you take a look at the hocon configurations for the [client](https://github.com/Lutando/Akkatecture/tree/master/examples/cluster/Akkatecture.Examples.ClusterClient/client.conf), the [worker](https://github.com/Lutando/Akkatecture/tree/master/examples/cluster/Akkatecture.Examples.Worker/worker.conf), and the [seed](https://github.com/Lutando/Akkatecture/tree/master/examples/cluster/Akkatecture.Examples.Seed/seed.conf), you will see that they all point to the same well known address that tells the actor system to use that address to initiate cluster gossip, and thus to establish the clustered network of actor systems. Each of the projects use the default akkatecture hocon configuration as a fallback configuration because this configuration has akkatecture opinionated "sane" defaults.

The sample is as easy as running all three projects at the same time. To interact with the domain just press enter on the clients console window, resulting in random commands being created, which are then fed through the cluster proxy that will serialize, deserialize, and route the commands to the necessary aggregate.