# learn-aws-cloudtrail-blue-team
how to read cloudtrail logs like a blue team

### Format files into JSON
```bash
for file in *.json; do jq . "$file" > "$file.tmp" && mv "$file.tmp" "$file"; done
```
### Check if new users are created
```bash
grep -r userName | sort -u
```
### Check if STS Get-Caller-Identity called
```bash
# Or statement
grep -rE 'temp-user|s3' . | sort -u
grep -rE 'temp-user|GetCallerIdentity' | sort -u
# Expand 
grep -h -A 10 'temp-user' | sort -u
```
