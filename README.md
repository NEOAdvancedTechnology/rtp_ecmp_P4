# rtp_ecmp_P4
ECMP based on RTP Sequence Number hash in P4

RTP_ECMP_P4.png is a Wireshark capture of an RTP stream with incrementing sequence numbers ingressing into the switch, and the packets being sent out of four different egress ports based on a hash of the RTP sequence number.
