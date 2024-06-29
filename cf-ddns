#!/bin/bash

# Check for current external IP
IP=`dig +short txt ch whoami.cloudflare @1.0.0.1| tr -d '"'`

# Set Cloudflare API
URL="https://api.cloudflare.com/client/v4/zones/DNS_ZONE_ID/dns_records/DNS_ENTRY_ID"
TOKEN="YOUR_TOKEN_HERE"
NAME="DNS_ENTRY_NAME"

# Connect to Cloudflare
cf() {
curl -X ${1} "${URL}" \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer ${TOKEN}" \
      ${2} ${3}
}

# Get current DNS data
RESULT=$(cf GET)
IP_CF=$(jq -r '.result.content' <<< ${RESULT})

# Compare IPs
if [ "$IP" = "$IP_CF" ]; then
    echo "No change."
else
    RESULT=$(cf PUT --data "{\"type\":\"A\",\"name\":\"${NAME}\",\"content\":\"${IP}\"}")
    echo "DNS updated."
fi
