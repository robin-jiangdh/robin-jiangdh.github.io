#AWS S3 Recipes
linkTitle: "AWS S3 Recipes"
weight: 100
description: >-
     AWS S3 Recipes
---

## Using AWS IAM — Identity and Access Management roles

For EC2 instance, there is an option to configure an IAM role:

![](select-ec2-iam-role.png)

Role shall contain a policy with permissions like:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "allow-put-and-get",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::BUCKET_NAME/test_s3_disk/*"
        }
    ]
}
```

Corresponding configuration of ClickHouse:

```
<clickhouse>
    <storage_configuration>
        <disks>
            <disk_s3>
                <type>s3</type>
                <endpoint>http://s3.us-east-1.amazonaws.com/BUCKET_NAME/test_s3_disk/</endpoint>
                <use_environment_credentials>true</use_environment_credentials>
            </disk_s3>
        </disks>
        <policies>
            <policy_s3_only>
                <volumes>
                    <volume_s3>
                        <disk>disk_s3</disk>
                    </volume_s3>
                </volumes>
            </policy_s3_only>
        </policies>
    </storage_configuration>
</clickhouse>
```

Small check:

```
CREATE TABLE table_s3 (number Int64) ENGINE=MergeTree() ORDER BY tuple() PARTITION BY tuple() SETTINGS storage_policy='policy_s3_only';
INSERT INTO table_s3 SELECT * FROM system.numbers LIMIT 100000000;
SELECT * FROM table_s3;
DROP TABLE table_s3;
```
