# Remote Denial of Service attack on full_nodes

## Issue Summary

Full_nodes connect with each other via a peer-2-peer protocol.

The connection established between nodes is encrypted and follows a protocol where a handshake is performed after which the connection is either disconnected or
remains open for further communication.

A Remote Denial of Service attack could create too many such connections for a full_node to process and close out before resources were depleted.

## Timeline (all times PST)  10/22/2022 - 02/15/2023. All times approximations

10/22/2022 - A researcher submitted a bug report via bugcrowd (https://bugcrowd.com/chia-network-bb) detailing the security issue.

02/15/2023 - Version 1.7.0 of the Chia blockchain was released that stops accepting new connections when a threshold is reached in the queue of connections that are being processed.

## Resolution and recovery

This issue was addressed in 1.7.0 by: Monitoring the queue of connections that are being processed and when a threshold is reached, stop accepting connections until there is room to accept additional connections again.

