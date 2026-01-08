# Orchestra

This repository contains the necessary files and documentation to configure server with Ansible.

## Prerequisites

Before getting started, make sure you have the following:

- Ansible installed on your local

## Getting Started

To create the server and configure it with Ansible, follow these steps:

run `ansible-playbook -i inventory/hosts web.yml` to configure it with Ansible.

## Adding Inventory/Hosts for Ansible

To add inventory/hosts for Ansible to work, follow these steps:

1. Open the `inventory/hosts` file in a text editor.
2. Add the IP address or hostname of the server under the appropriate group.
3. Save the file.

Now you can use Ansible to manage and configure the server using the added inventory/hosts.

## Exposed Ports

The following table lists the default ports exposed by the services:

| Service         | Port(s)                       | Description                                                     |
| :-------------- | :---------------------------- | :-------------------------------------------------------------- |
| Jenkins         | 8080                          | Web UI                                                          |
| Kafka           | 9092, 9101                    | 9092 (Broker), 9101 (JMX for monitoring)                        |
| Kafka UI        | 8080                          | Web UI                                                          |
| Jaeger          | 16686                         | Web UI / Frontend                                               |
|                 | 14268                         | Collector (Jaeger Thrift over HTTP)                             |
|                 | 6831/udp                      | Agent (Jaeger Thrift over UDP)                                  |
|                 | 6832/udp                      | Agent (Internal)                                                |
|                 | 5778                          | Agent (Config service)                                          |
|                 | 5775/udp                      | Agent (Zipkin format)                                           |
|                 | 9411                          | Collector (Zipkin format)                                       |
|                 | 4317                          | Collector (OTLP over gRPC)                                      |
|                 | 4318                          | Collector (OTLP over HTTP)                                      |
| MinIO           | 9005, 9006                    | 9005 (API), 9006 (Console)                                      |
| Portainer       | 8000, 9000, 9443              | 8000 (Edge Agent), 9000 (HTTP), 9443 (HTTPS)                    |
| Redis           | 6379                          | Redis Server                                                    |
| Redis Insight   | 8001                          | Web UI                                                          |

