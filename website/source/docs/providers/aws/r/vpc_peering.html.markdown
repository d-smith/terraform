---
layout: "aws"
page_title: "AWS: aws_vpc_peering_connection"
sidebar_current: "docs-aws-resource-vpc-peering"
description: |-
  Provides an VPC Peering Connection resource.
---

# aws\_vpc\_peering\_connection

Provides an VPC Peering Connection resource.

## Example Usage

Basic usage:

```
resource "aws_vpc_peering_connection" "foo" {
    peer_owner_id = "${var.peer_owner_id}"
    peer_vpc_id = "${aws_vpc.bar.id}"
    vpc_id = "${aws_vpc.foo.id}"
}
```

Basic usage with connection options:

```
resource "aws_vpc_peering_connection" "foo" {
    peer_owner_id = "${var.peer_owner_id}"
    peer_vpc_id = "${aws_vpc.bar.id}"
    vpc_id = "${aws_vpc.foo.id}"

    accepter {
      allow_remote_vpc_dns_resolution = true
    }

    requester {
      allow_remote_vpc_dns_resolution = true
    }
}
```

Basic usage with tags:

```

resource "aws_vpc_peering_connection" "foo" {
    peer_owner_id = "${var.peer_owner_id}"
    peer_vpc_id = "${aws_vpc.bar.id}"
    vpc_id = "${aws_vpc.foo.id}"

    auto_accept = true

    tags {
      Name = "VPC Peering between foo and bar"
    }
}

resource "aws_vpc" "foo" {
    cidr_block = "10.1.0.0/16"
}

resource "aws_vpc" "bar" {
    cidr_block = "10.2.0.0/16"
}
```

## Argument Reference

The following arguments are supported:

* `peer_owner_id` - (Required) The AWS account ID of the owner of the peer VPC.
* `peer_vpc_id` - (Required) The ID of the VPC with which you are creating the VPC Peering Connection.
* `vpc_id` - (Required) The ID of the requester VPC.
* `auto_accept` - (Optional) Accept the peering (you need to be the owner of both VPCs).
* `accepter` (Optional) - An optional configuration block that allows for [VPC Peering Connection]
(http://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide) options to be set for the VPC that accepts
the peering connection (a maximum of one).
* `requester` (Optional) - A optional configuration block that allows for [VPC Peering Connection]
(http://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide) options to be set for the VPC that requests
the peering connection (a maximum of one).
* `tags` - (Optional) A mapping of tags to assign to the resource.

#### Accepter and Requester Arguments

-> **Note:** When enabled, the DNS resolution feature requires that VPCs participating in the peering
must have support for the DNS hostnames enabled. This can be done using the [`enable_dns_hostnames`]
(vpc.html#enable_dns_hostnames) attribute in the [`aws_vpc`](vpc.html) resource. See [Using DNS with Your VPC]
(http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-dns.html) user guide for more information.

* `allow_remote_vpc_dns_resolution` - (Optional) Allow a local VPC to resolve public DNS hostnames to private
IP addresses when queried from instances in the peer VPC.
* `allow_classic_link_to_remote_vpc` - (Optional) Allow a local linked EC2-Classic instance to communicate
with instances in a peer VPC. This enables an outbound communication from the local ClassicLink connection
to the remote VPC.
* `allow_vpc_to_remote_classic_link` - (Optional) Allow a local VPC to communicate with a linked EC2-Classic
instance in a peer VPC. This enables an outbound communication from the local VPC to the remote ClassicLink
connection.

## Attributes Reference

The following attributes are exported:

* `id` - The ID of the VPC Peering Connection.
* `accept_status` - The status of the VPC Peering Connection request.


## Notes

If you are not the owner of both VPCs, or do not enable the `auto_accept` attribute you will still
have to accept the VPC Peering Connection request manually using the AWS Management Console, AWS CLI,
through SDKs, etc.

## Import

VPC Peering resources can be imported using the `vpc peering id`, e.g.

```
$ terraform import aws_vpc_peering_connection.test_connection pcx-111aaa111
```
