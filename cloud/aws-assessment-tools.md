# AWS Assessment Tools

### Cloudmapper

```text
git clone https://github.com/duo-labs/cloudmapper.git
brew install autoconf automake libtool jq awscli python3 pipenv
cd cloudmapper/
pipenv install --skip-lock
pipenv shell
# Adjust your config.json
python cloudmapper.py collect --config config.json --account spaceteam
python cloudmapper.py weboftrust --account spaceteam 
python cloudmapper.py iam_report --accounts spaceteam
```

### Prowler

```text
git clone https://github.com/toniblyx/prowler
cd prowler
./prowler -sn | aha > prowler-report.htm
```

### PMapper

```text
https://github.com/nccgroup/PMapper.git
cd PMapper
pip install -r requirements.txt

# Use my automated PMapper script
# Create graph only
aws_privilege_audit.py graph
# Create graph and visualize
aws_privilege_audit.py start
# Identify privilege escalation opportunities
aws_privilege_audit.py escalate

# Create pretty graph
brew install librsvg
rsvg-convert -h 4096 pmapper-viz-acct-12345678910.svg > pmapper-viz-this-acct.png 
```

## 

