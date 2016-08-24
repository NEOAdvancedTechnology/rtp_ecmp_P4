# rtp_ecmp_P4
ECMP based on RTP Sequence Number hash in P4

RTP_ECMP_P4.png is a Wireshark capture of an RTP stream with incrementing sequence numbers ingressing into the switch, and the packets being sent out of four different egress ports based on a hash of the RTP sequence number.

The code is based on p4lang/tutorials/master/examples/action_profile, with the addition of a UDP and RTP header parser, and changing the hash field to rtp.sequence_number.

### Explanations of commands_rtp_ecmp.txt

```
01. table_indirect_create_group ecmp_group
02. table_indirect_create_member ecmp_group _nop
03. table_indirect_create_member ecmp_group set_ecmp_nexthop 1
04. table_indirect_create_member ecmp_group set_ecmp_nexthop 2
05. table_indirect_create_member ecmp_group set_ecmp_nexthop 3
06. table_indirect_add_member_to_group ecmp_group 1 0
07. table_indirect_add_member_to_group ecmp_group 2 0
08. table_indirect_add_member_to_group ecmp_group 3 0
09. table_indirect_set_default ecmp_group 0
10. table_indirect_add_with_group ecmp_group 239.1.1.1 => 0
11. table_dump ecmp_group
```

These commands do the following:

01. create a group, which receives group handle 0
02. create a member with action _nop and no action data (member handle 0)
03. create a member with action set_ecmp_nexthop and action data port=1 (member handle 1)
04. create a member with action set_ecmp_nexthop and action data port=2 (member handle 2)
05. create a member with action set_ecmp_nexthop and action data port=3 (member handle 3)
06. add member 1 to group 0
07. add member 2 to group 0
08. add member 3 to group 0
09. set member 0 (_nop) has the default for table ecmp_group
10. add an entry to table ecmp_group, which maps destination IPv4 address
    239.1.1.1 to group 0 (which includes members 1, 2, and 3)
11. simply dump the table
