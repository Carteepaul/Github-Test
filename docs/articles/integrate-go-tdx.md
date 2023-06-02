---
title: Intel TDX Client
description: Developer documentation for the Intel TDX attestation client and CLI.
author: grminch
topic: integrate
date: 01/06/2023
---

# Project Amber Go Intel® TDX adapter

The Project Amber Intel® TDX Attestation client includes a Go client and a CLI for use on the command line or by languages other than Go, enabling you to use TDX remote attestation in your custom application. 

## Intel® TDX interfaces

The following APIs are exposed by `go-tdx`.

**In this section**

[tdx.NewAdapter](#newadapter): Initializes an instance of TdxAdapter, which manages the quote collection from the TD (trusted domain) by invoking DCAP quote generation APIs. The TdxAdapter also collects event log data for the TD.
  
[tdx.CollectEvidence](#collectevidence): Interfaces with the TDX Attestation library to get the TD quote by calling into the DCAP Quote Generation Service running on the host.

[tdx.Decrypt](#decrypt): Takes encrypted data as input and decrypts the data using the private key in _EncryptionMetadata_.
  
[tdx.NewEventLogParser](#neweventlogparser): Initializes an instance of EventLogParser which manages the event log collection from TD by reading either the ACPI tables or an EFI file containing event log data.
  
[tdx.GetEventLogs](#geteventlogs): The GetEventLogs API returns a TD event log in the form of JSON. The TD event log consists of all the events that get measured to all the four RTMRs in order.  

### NewAdapter

**NewAdapter** initializes an instance of TdxAdapter, which manages the quote collection from a trust domain (TD) by invoking low level quote generation APIs. TdxAdapter is also responsible for collecting the event log for a TD.

The following code snippet shows how to create a new Go TDX adapter, and then use the adapter to collect a quote from the TDX-enabled platform.

```go
import "github.com/intel/amber-client/go-tdx"

evLogParser := tdx.NewEventLogParser()
adapter, err := tdx.NewAdapter(tdHeldData, evLogParser)
if err != nil {
    return err
}

evidence, err := adapter.CollectEvidence(nonce)
if err != nil {
    return err
}
```

### CollectEvidence

_CollectEvidence_  interfaces with TDX Attestation library to get the TD quote by calling into the DCAP Quote Generation Service running on the host. It takes a nonce as input, which is hashed during TD report creation. CollectEvidence returns an Evidence structure which contains Quote, UserData and Event log in case of success, else error in case of failure.

```go
CollectEvidence(nonce []byte) (*client.Evidence, error)
```

### Decrypt

_Decrypt_ takes encrypted data as input and decrypts the data using the private key that is passed in the EncryptionMetadata. The Decrypt API returns a byte array containing decrypted data in case of success, else error in case of failure.

```go
Decrypt(encryptedData []byte, em *EncryptionMetadata) ([]byte, error)
```

The following snippet shows how to decrypt an encrypted blob.

```go
em := &tdx.EncryptionMetadata{
	PrivateKeyLocation: privateKeyPath,
}
decryptedData, err := tdx.Decrypt(encryptedData, em)
if err != nil {
    fmt.Printf("Something bad happened: %s\n\n", err)
     return err
}
```

### NewEventLogParser

Initializes an instance of EventLogParser which manages the event log collection from TD by reading either the ACPI tables or an EFI file containing event log data. The event log for a TD is stored in ACPI tables.

See the code snippet in [GetEventLogs](#geteventlogs) for sample usage.

### GetEventLogs 

The GetEventLogs API returns a TD event log in the form of JSON. TD event log consists of all the events that gets measured to all the four RTMRs in order. TD event log can be used to verify the integrity of RTMRs by replaying all the events measurements in order.

```go
GetEventLogs() ([]RtmrEventLog, error)
```

The following snippet collects the event log from a TD.

```go
evLogParser := tdx.NewEventLogParser()
eventLog, err := evLogParser.GetEventLogs()
if err != nil {
    return err
}
```
