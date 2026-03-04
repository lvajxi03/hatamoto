+++
date = '2025-12-11T15:11:23+02:00'
title = 'VPC'
menus = 'main'
type = 'page'
+++

## AWS VPC

`VPC` stands for `Virtual Private Cloud`

### Create VPC

```sh
aws ec2 create-vpc \
  --cidr-block 10.0.0.0/16 \
  --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=MyVPC}]'
```

