#Robin Cloud Access Management
linkTitle: "Robin Cloud Access Management"
description: >
    Enabling access_management for Robin.Cloud databases.
weight: 5
---
Organizations that want to enable administrative users in their Robin.Cloud ClickHouse servers can do so by enabling `access_management` manually.  This allows for administrative users to be created on the specific ClickHouse Cluster.


Modifying the ClickHouse cluster settings manually can lead to the cluster not loading or other issues.  Change settings only with full consultation with an Robin.Cloud support team member, and be ready to remove settings if they cause any disruption of service.


To add the `access_management` setting to an Robin.Cloud ClickHouse Cluster:

1. Log into your Robin.Cloud account.
1. For the cluster to modify, select **Configure -> Settings**.

    {{< figure src="/assets/Robin-cloud-cluster-settings-configure.png" width="400" title="Cluster setting configure" >}}

1. From the Settings page, select **+ADD SETTING**.

    {{< figure src="/assets/Robin-cloud-cluster-add-setting.png" title="Add cluster setting" >}}

1. Set the following options:
    1. **Setting Type**:  Select **users.d file**.
    1. **Filename**: `access_management.xml`
    1. **Contents**:  Enter the following to allow the `clickhouse_operator` that controls the cluster through the `clickhouse-operator` the ability to set administrative options:

        ```
        <yandex>
            <users>
                <admin>
                    <access_management>1</access_management>
                </admin>
                <clickhouse_operator>
                    <access_management>1</access_management>
                </clickhouse_operator>
            </users>
        </yandex>
        ```

    access_management=1 means that users `admin`, `clickhouse_operator` are able to create users and grant them privileges using SQL.

1. Select **OK**.  The cluster will restart, and users can now be created in the cluster that can be granted administrative access.

1. If you are running ClickHouse 21.9 and above you can enable storing access management in ZooKeeper. in this case it will be automatically propagated to the cluster. This requires yet another configuration file:
    1. **Setting Type**: Select **config.d file**
    2. **Filename**: `user_directories.xml`
    3. **Contents**:
    
       ```
       <yandex>
         <user_directories replace="replace">
           <users_xml>
             <path>/etc/clickhouse-server/users.xml</path>
           </users_xml>
           <replicated>
             <zookeeper_path>/clickhouse/access/</zookeeper_path>
           </replicated>
         </user_directories>
       </yandex>
       ```
