# learn-aws-cloudtrail-blue-team
how to read cloudtrail logs like a blue team

Download sample json file
```bash
wget -O custom_filename.zip "https://cdn.discordapp.com/attachments/1145109454346522654/1146038520868245526/INCIDENT-3252.zip?ex=677084cb&is=676f334b&hm=a0addc7846dc7c7886c1c863b690e78134173f867f44887e58e014e767edb675"
```
### Format files into JSON
```bash
for file in *.json; do jq . "$file" > "$file.tmp" && mv "$file.tmp" "$file"; done
```
### Check if new users are created
```bash
grep -r userName | sort -u
```
### Check if STS Get-Caller-Identity called and check ip address and which aws account it came from
Identify iam users that are out of the ordinary
```bash
# OR statement
grep -rE 'temp-user|GetCallerIdentity' | sort -u
curl ipinfo.io/<ip address>
# AND statement
grep -rE "temp-user" && grep -rE "s3" | sort -u
# Expand 
grep -h -A 10 'temp-user' <file> | sort -u
```
### Check if files were pull from s3 buckets
```bash
grep -rE 'temp-user|s3|ListObjects' . | sort -u
```
### If user doesn't have elevated policy, check if AssumeRole is next
```bash
grep -rE "temp-user" && grep -rE "AssumeRole" | sort -u
```
