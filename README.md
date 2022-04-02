# CVSAP 

CVS Adapter Protocol
-------

## Origin

In a project, `GIT` `SVN` and other version control systems need to be integrated. It is found that the existing relevant protocols fight their own battles and are not friendly to integrating a variety of version control systems. This idea came into being after referring to `vscode` syntax adapter protocol `LSP` and debugging adapter protocol `DAP`. The workload of integrating many subsystems is huge. Anyone is welcome to join the project. However, this document is only limited to the creation of the agreement.

## Specification

This specification is based on `JSON-RPC` Protocol, contains two roles from:

 - [Client Specification](docs/ClientSpecification.md)
 - [Server Specification](docs/ServerSpecification.md)

