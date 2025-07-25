# Palo Alto Network Integration for Elastic

## Overview

The Palo Alto Network Integration for Elastic enables collection of logs from Palo Alto Networks' PAN-OS firewalls. This integration facilitates real-time visibility into network
activity, threat detection and security operations.

### Compatibility

This integration is compatible with PAN-OS versions 10.2, 11.1 and 11.2.

Support for specific log types varies by PAN-OS version. GlobalProtect logs are supported starting with PAN-OS version 9.1.3. User-ID logs are supported for PAN-OS version 8.1 and
above, while Tunnel Inspection logs are supported for version 9.1 and later.

This integration can receive logs from syslog via TCP or UDP, or read from log files.

## What data does this integration collect?

The Palo Alto Network integration collects log messages of the following types:

* [GlobalProtect](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/globalprotect-log-fields.html)
* [HIP Match](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/hip-match-log-fields.html)
* [Threat](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/threat-log-fields.html)
* [Traffic](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/traffic-log-fields.html)
* [User-ID](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/user-id-log-fields.html)
* [Authentication](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/authentication-log-fields)
* [Config](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/config-log-fields)
* [Correlated Events](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/correlated-events-log-fields)
* [Decryption](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/decryption-log-fields)
* [GTP](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/gtp-log-fields)
* [IP-Tag](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/ip-tag-log-fields)
* [SCTP](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/sctp-log-fields)
* [System](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/system-log-fields)
* [Tunnel Inspection](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/tunnel-inspection-log-fields).

### Supported use cases

Integrating Palo Alto Networks (PANW) with the Elastic Stack creates a powerful solution for transforming raw firewall logs into actionable intelligence, dramatically enhancing
security and operational visibility. This synergy enables advanced use cases including real-time threat detection and hunting through Elastic SIEM, deep network traffic analysis
with intuitive Kibana dashboards, and automated incident response by connecting with Cortex XSOAR. By centralizing and analyzing PANW data, organizations can strengthen their
security posture, optimize network performance, and build a solid data foundation for implementing a Zero Trust architecture.

## What do I need to use this integration?

Elastic Agent must be installed. For more details, check the Elastic Agent [installation instructions](docs-content://reference/fleet/install-elastic-agents.md). You can install only one Elastic Agent per host.

Elastic Agent is required to stream data from the syslog or log file receiver and ship the data to Elastic, where the events will then be processed via the integration's ingest pipelines.

## How do I deploy this integration?

### Collect logs via syslog

To configure syslog monitoring, follow the steps described in the [Configure Syslog Monitoring](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/use-syslog-for-monitoring/configure-syslog-monitoring) documentation.

### Collect logs via log file

To configure log file monitoring, follow the steps described in the [Configure Log Forwarding](https://docs.paloaltonetworks.com/pan-os/10-2/pan-os-admin/monitoring/configure-log-forwarding) documentation.

### Enable the integration in Elastic

1. In Kibana navigate to **Management** > **Integrations**.
2. In the search bar, type **Palo Alto Next-Gen Firewall**.
3. Select the **Palo Alto Next-Gen Firewall** integration and add it.
4. If needed, install Elastic Agent on the systems which receive syslog messages or log files.
5. Enable and configure only the collection methods which you will use.

    * **To collect logs via syslog over TCP**, you'll need to configure the syslog server host and port details.

    * **To collect logs via syslog over UDP**, you'll need to configure the syslog server host and port details.

    * **To collect logs via log file**, configure the file path patterns which will be monitored, in the Paths field.

6. Press **Save Integration** to begin collecting logs.

### Validate log collection

1. In Kibana, navigate to **Dashboards**.
2. In the search bar, type **Logs PANW**.
3. Select a dashboard overview for the data type you are collecting, and verify the dashboard information is populated.

## Troubleshooting

For help with Elastic ingest tools, check [Common problems](https://www.elastic.co/docs/troubleshoot/ingest/fleet/common-problems).

If events are truncated, increase `max_message_size` option for TCP and UDP input type. You can find it under Advanced Options and configure it as per requirements.
The default value of `max_message_size` is set to 50KiB.

If the TCP input is used, it is recommended that PAN-OS is configured to send syslog messages using the IETF (RFC 5424) format. In addition, RFC 6587 framing (Octet Counting) will
be enabled by default on the TCP input.

To verify the configuration before and after the change (fields `before-change-detail` and `after-change-detail`) in the [config-log](https://docs.paloaltonetworks.com/pan-os/11-1/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/config-log-fields), use the following [custom log format in the syslog server profile](https://docs.paloaltonetworks.com/pan-os/11-1/pan-os-admin/monitoring/use-syslog-for-monitoring/syslog-field-descriptions/custom-logevent-format):
  ``1,$receive_time,$serial,$type,$subtype,2561,$time_generated,$host,$vsys,$cmd,$admin,$client,$result,$path,$before-change-detail,$after-change-detail,$seqno,$actionflags,$dg_hier_level_1,$dg_hier_level_2,$dg_hier_level_3,$dg_hier_level_4,$vsys_name,$device_name,$dg_id,$comment,0,$high_res_timestamp``

## Performance and scaling

For more information on architectures that can be used for scaling this integration, check the [Ingest Architectures](https://www.elastic.co/docs/manage-data/ingest/ingest-reference-architectures) documentation.

## Reference

### ECS field reference

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| client.bytes | Bytes sent from the client to the server. | long |
| client.ip | IP address of the client (IPv4 or IPv6). | ip |
| client.nat.ip | Translated IP of source based NAT sessions (e.g. internal client to internet). Typically connections traversing load balancers, firewalls, or routers. | ip |
| client.nat.port | Translated port of source based NAT sessions (e.g. internal client to internet). Typically connections traversing load balancers, firewalls, or routers. | long |
| client.packets | Packets sent from the client to the server. | long |
| client.port | Port of the client. | long |
| client.user.name | Short name or login of the user. | keyword |
| client.user.name.text | Multi-field of `client.user.name`. | match_only_text |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |
| cloud.image.id | Image ID for the cloud instance. | keyword |
| cloud.instance.id | Instance ID of the host machine. | keyword |
| cloud.instance.name | Instance name of the host machine. | keyword |
| cloud.machine.type | Machine type of the host machine. | keyword |
| cloud.project.id | Name of the project in Google Cloud. | keyword |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |
| cloud.region | Region in which this host is running. | keyword |
| container.id | Unique container id. | keyword |
| container.image.name | Name of the image the container was built on. | keyword |
| container.labels | Image labels. | object |
| container.name | Container name. | keyword |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| destination.address | Some event destination addresses are defined ambiguously. The event will sometimes list an IP, a domain or a unix socket.  You should always store the raw address in the `.address` field. Then it should be duplicated to `.ip` or `.domain`, depending on which one it is. | keyword |
| destination.as.number | Unique number allocated to the autonomous system. The autonomous system number (ASN) uniquely identifies each network on the Internet. | long |
| destination.as.organization.name | Organization name. | keyword |
| destination.as.organization.name.text | Multi-field of `destination.as.organization.name`. | match_only_text |
| destination.bytes | Bytes sent from the destination to the source. | long |
| destination.domain | The domain name of the destination system. This value may be a host name, a fully qualified domain name, or another host naming format. The value may derive from the original event or be added from enrichment. | keyword |
| destination.geo.city_name | City name. | keyword |
| destination.geo.continent_name | Name of the continent. | keyword |
| destination.geo.country_iso_code | Country ISO code. | keyword |
| destination.geo.country_name | Country name. | keyword |
| destination.geo.location | Longitude and latitude. | geo_point |
| destination.geo.name | User-defined description of a location, at the level of granularity they care about. Could be the name of their data centers, the floor number, if this describes a local physical entity, city names. Not typically used in automated geolocation. | keyword |
| destination.geo.region_iso_code | Region ISO code. | keyword |
| destination.geo.region_name | Region name. | keyword |
| destination.ip | IP address of the destination (IPv4 or IPv6). | ip |
| destination.nat.ip | Translated ip of destination based NAT sessions (e.g. internet to private DMZ) Typically used with load balancers, firewalls, or routers. | ip |
| destination.nat.port | Port the source session is translated to by NAT Device. Typically used with load balancers, firewalls, or routers. | long |
| destination.packets | Packets sent from the destination to the source. | long |
| destination.port | Port of the destination. | long |
| destination.user.domain | Name of the directory the user is a member of. For example, an LDAP or Active Directory domain name. | keyword |
| destination.user.email | User email address. | keyword |
| destination.user.name | Short name or login of the user. | keyword |
| destination.user.name.text | Multi-field of `destination.user.name`. | match_only_text |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |
| error.message | Error message. | match_only_text |
| event.action | The action captured by the event. This describes the information in the event. It is more specific than `event.category`. Examples are `group-add`, `process-started`, `file-created`. The value is normally defined by the implementer. | keyword |
| event.category | This is one of four ECS Categorization Fields, and indicates the second level in the ECS category hierarchy. `event.category` represents the "big buckets" of ECS categories. For example, filtering on `event.category:process` yields all events relating to process activity. This field is closely related to `event.type`, which is used as a subcategory. This field is an array. This will allow proper categorization of some events that fall in multiple categories. | keyword |
| event.code | Identification code for this event, if one exists. Some event sources use event codes to identify messages unambiguously, regardless of message language or wording adjustments over time. An example of this is the Windows Event ID. | keyword |
| event.created | `event.created` contains the date/time when the event was first read by an agent, or by your pipeline. This field is distinct from `@timestamp` in that `@timestamp` typically contain the time extracted from the original event. In most situations, these two timestamps will be slightly different. The difference can be used to calculate the delay between your source generating an event, and the time when your agent first processed it. This can be used to monitor your agent's or pipeline's ability to keep up with your event source. In case the two timestamps are identical, `@timestamp` should be used. | date |
| event.dataset | Event dataset. | constant_keyword |
| event.duration | Duration of the event in nanoseconds. If `event.start` and `event.end` are known this value should be the difference between the end and start time. | long |
| event.end | `event.end` contains the date when the event ended or when the activity was last observed. | date |
| event.kind | This is one of four ECS Categorization Fields, and indicates the highest level in the ECS category hierarchy. `event.kind` gives high-level information about what type of information the event contains, without being specific to the contents of the event. For example, values of this field distinguish alert events from metric events. The value of this field can be used to inform how these kinds of events should be handled. They may warrant different retention, different access control, it may also help understand whether the data is coming in at a regular interval or not. | keyword |
| event.module | Event module. | constant_keyword |
| event.original | Raw text message of entire event. Used to demonstrate log integrity or where the full log message (before splitting it up in multiple parts) may be required, e.g. for reindex. This field is not indexed and doc_values are disabled. It cannot be searched, but it can be retrieved from `_source`. If users wish to override this and index this field, please see `Field data types` in the `Elasticsearch Reference`. | keyword |
| event.outcome | This is one of four ECS Categorization Fields, and indicates the lowest level in the ECS category hierarchy. `event.outcome` simply denotes whether the event represents a success or a failure from the perspective of the entity that produced the event. Note that when a single transaction is described in multiple events, each event may populate different values of `event.outcome`, according to their perspective. Also note that in the case of a compound event (a single event that contains multiple logical events), this field should be populated with the value that best captures the overall success or failure from the perspective of the event producer. Further note that not all events will have an associated outcome. For example, this field is generally not populated for metric events, events with `event.type:info`, or any events for which an outcome does not make logical sense. | keyword |
| event.reason | Reason why this event happened, according to the source. This describes the why of a particular action or outcome captured in the event. Where `event.action` captures the action from the event, `event.reason` describes why that action was taken. For example, a web proxy with an `event.action` which denied the request may also populate `event.reason` with the reason why (e.g. `blocked site`). | keyword |
| event.severity | The numeric severity of the event according to your event source. What the different severity values mean can be different between sources and use cases. It's up to the implementer to make sure severities are consistent across events from the same source. The Syslog severity belongs in `log.syslog.severity.code`. `event.severity` is meant to represent the severity according to the event source (e.g. firewall, IDS). If the event source does not publish its own severity, you may optionally copy the `log.syslog.severity.code` to `event.severity`. | long |
| event.start | `event.start` contains the date when the event started or when the activity was first observed. | date |
| event.timezone | This field should be populated when the event's timestamp does not include timezone information already (e.g. default Syslog timestamps). It's optional otherwise. Acceptable timezone formats are: a canonical ID (e.g. "Europe/Amsterdam"), abbreviated (e.g. "EST") or an HH:mm differential (e.g. "-05:00"). | keyword |
| event.type | This is one of four ECS Categorization Fields, and indicates the third level in the ECS category hierarchy. `event.type` represents a categorization "sub-bucket" that, when used along with the `event.category` field values, enables filtering events down to a level appropriate for single visualization. This field is an array. This will allow proper categorization of some events that fall in multiple event types. | keyword |
| file.name | Name of the file including the extension, without the directory. | keyword |
| file.path | Full path to the file, including the file name. It should include the drive letter, when appropriate. | keyword |
| file.path.text | Multi-field of `file.path`. | match_only_text |
| file.type | File type (file, dir, or symlink). | keyword |
| host.architecture | Operating system architecture. | keyword |
| host.containerized | If the host is a container. | boolean |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |
| host.ip | Host ip addresses. | ip |
| host.mac | Host MAC addresses. The notation format from RFC 7042 is suggested: Each octet (that is, 8-bit byte) is represented by two [uppercase] hexadecimal digits giving the value of the octet as an unsigned integer. Successive octets are separated by a hyphen. | keyword |
| host.name | Name of the host. It can contain what hostname returns on Unix systems, the fully qualified domain name (FQDN), or a name specified by the user. The recommended value is the lowercase FQDN of the host. | keyword |
| host.os.build | OS build information. | keyword |
| host.os.codename | OS codename, if any. | keyword |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |
| host.os.full | Operating system name, including the version or code name. | keyword |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |
| host.os.name | Operating system name, without the version. | keyword |
| host.os.name.text | Multi-field of `host.os.name`. | text |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |
| host.os.version | Operating system version as a raw string. | keyword |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |
| hostname | Name of host parsed from syslog message. | keyword |
| http.request.method | HTTP request method. The value should retain its casing from the original event. For example, `GET`, `get`, and `GeT` are all considered valid values for this field. | keyword |
| http.request.referer | Referrer for this HTTP request. | keyword |
| http.request.referrer | Referrer for this HTTP request. | keyword |
| http.version | HTTP version. | keyword |
| input.type | Type of Filebeat input. | keyword |
| labels | Custom key/value pairs. Can be used to add meta information to events. Should not contain nested objects. All values are stored as keyword. Example: `docker` and `k8s` labels. | object |
| labels.captive_portal |  | boolean |
| labels.client_server_policy_based_forwarding |  | boolean |
| labels.connect_to_destination_host |  | boolean |
| labels.container_page |  | boolean |
| labels.decrypted_traffic |  | boolean |
| labels.enterprise_credential_submission |  | boolean |
| labels.file_submitted_to_WildFire |  | boolean |
| labels.http_proxy |  | boolean |
| labels.ipv6_session |  | boolean |
| labels.nat_translated |  | boolean |
| labels.non_standard_port_usage |  | boolean |
| labels.payload_outer_tunnel |  | boolean |
| labels.pcap_included |  | boolean |
| labels.server_client_policy_based_forwarding |  | boolean |
| labels.source_flow_allow_list |  | boolean |
| labels.ssl_decrypted |  | boolean |
| labels.symmetric_return |  | boolean |
| labels.temporary_match |  | boolean |
| labels.url_filter_denied |  | boolean |
| labels.x_forwarded_for |  | boolean |
| log.file.path | Path to the log file. | keyword |
| log.flags | Flags for the log file. | keyword |
| log.level | Original log level of the log event. If the source of the event provides a log level or textual severity, this is the one that goes in `log.level`. If your source doesn't specify one, you may put your event transport's severity here (e.g. Syslog severity). Some examples are `warn`, `err`, `i`, `informational`. | keyword |
| log.offset | Offset of the entry in the log file. | long |
| log.source.address | Source address from which the log event was read / sent from. | keyword |
| log.syslog.facility.code | The Syslog numeric facility of the log event, if available. According to RFCs 5424 and 3164, this value should be an integer between 0 and 23. | long |
| log.syslog.facility.name | The Syslog text-based facility of the log event, if available. | keyword |
| log.syslog.hostname | The hostname, FQDN, or IP of the machine that originally sent the Syslog message. This is sourced from the hostname field of the syslog header. Depending on the environment, this value may be different from the host that handled the event, especially if the host handling the events is acting as a collector. | keyword |
| log.syslog.priority | Syslog numeric priority of the event, if available. According to RFCs 5424 and 3164, the priority is 8 \* facility + severity. This number is therefore expected to contain a value between 0 and 191. | long |
| log.syslog.severity.code | The Syslog numeric severity of the log event, if available. If the event source publishing via Syslog provides a different numeric severity value (e.g. firewall, IDS), your source's numeric severity should go to `event.severity`. If the event source does not specify a distinct severity, you can optionally copy the Syslog severity to `event.severity`. | long |
| log.syslog.severity.name | The Syslog numeric severity of the log event, if available. If the event source publishing via Syslog provides a different severity value (e.g. firewall, IDS), your source's text severity should go to `log.level`. If the event source does not specify a distinct severity, you can optionally copy the Syslog severity to `log.level`. | keyword |
| log.syslog.version | The version of the Syslog protocol specification. Only applicable for RFC 5424 messages. | keyword |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |
| network.application | When a specific application or service is identified from network connection details (source/dest IPs, ports, certificates, or wire format), this field captures the application's or service's name. For example, the original event identifies the network connection being from a specific web service in a `https` network connection, like `facebook` or `twitter`. The field value must be normalized to lowercase for querying. | keyword |
| network.bytes | Total bytes transferred in both directions. If `source.bytes` and `destination.bytes` are known, `network.bytes` is their sum. | long |
| network.community_id | A hash of source and destination IPs and ports, as well as the protocol used in a communication. This is a tool-agnostic standard to identify flows. Learn more at https://github.com/corelight/community-id-spec. | keyword |
| network.direction | Direction of the network traffic. When mapping events from a host-based monitoring context, populate this field from the host's point of view, using the values "ingress" or "egress". When mapping events from a network or perimeter-based monitoring context, populate this field from the point of view of the network perimeter, using the values "inbound", "outbound", "internal" or "external". Note that "internal" is not crossing perimeter boundaries, and is meant to describe communication between two hosts within the perimeter. Note also that "external" is meant to describe traffic between two hosts that are external to the perimeter. This could for example be useful for ISPs or VPN service providers. | keyword |
| network.forwarded_ip | Host IP address when the source IP address is the proxy. | ip |
| network.packets | Total packets transferred in both directions. If `source.packets` and `destination.packets` are known, `network.packets` is their sum. | long |
| network.transport | Same as network.iana_number, but instead using the Keyword name of the transport layer (udp, tcp, ipv6-icmp, etc.) The field value must be normalized to lowercase for querying. | keyword |
| network.type | In the OSI Model this would be the Network Layer. ipv4, ipv6, ipsec, pim, etc The field value must be normalized to lowercase for querying. | keyword |
| observer.egress.interface.name | Interface name as reported by the system. | keyword |
| observer.egress.zone | Network zone of outbound traffic as reported by the observer to categorize the destination area of egress traffic, e.g. Internal, External, DMZ, HR, Legal, etc. | keyword |
| observer.geo.name | User-defined description of a location, at the level of granularity they care about. Could be the name of their data centers, the floor number, if this describes a local physical entity, city names. Not typically used in automated geolocation. | keyword |
| observer.hostname | Hostname of the observer. | keyword |
| observer.ingress.interface.name | Interface name as reported by the system. | keyword |
| observer.ingress.zone | Network zone of incoming traffic as reported by the observer to categorize the source area of ingress traffic. e.g. internal, External, DMZ, HR, Legal, etc. | keyword |
| observer.product | The product name of the observer. | keyword |
| observer.serial_number | Observer serial number. | keyword |
| observer.type | The type of the observer the data is coming from. There is no predefined list of observer types. Some examples are `forwarder`, `firewall`, `ids`, `ips`, `proxy`, `poller`, `sensor`, `APM server`. | keyword |
| observer.vendor | Vendor name of the observer. | keyword |
| panw.panos.access_point.name | Reference to a Packet Data Network Data Gateway (PGW)/ Gateway GPRS Support Node in a mobile network. Composed of a mandatory APN Network Identifier and an optional APN Operator Identifier. | keyword |
| panw.panos.action | Action taken for the session; values are alert, allow, deny, drop, drop-all-packets, reset-client, reset-server, reset-both, block-url. | keyword |
| panw.panos.action_flags | A bit field indicating if the log was forwarded to Panorama. | keyword |
| panw.panos.action_source | Specifies whether the action taken to allow or block an application was defined in the application or in policy. The actions can be allow, deny, drop, reset- server, reset-client or reset-both for the session. | keyword |
| panw.panos.admin | Username of the Administrator performing the configuration. | keyword |
| panw.panos.after_change_detail | This field is in custom logs only; it is not in the default format.It contains the full xpath after the configuration change. | match_only_text |
| panw.panos.application.category | The application category specified in the application configuration properties. Values are: business-systems, collaboration, general-internet, media, networking, saas. | keyword |
| panw.panos.application.characteristics | Comma-separated list of applicable characteristic of the application. | keyword |
| panw.panos.application.container | The parent application for an application. | keyword |
| panw.panos.application.is_saas | Displays 1 if a SaaS application or 0 if not a SaaS application. | keyword |
| panw.panos.application.is_sanctioned | Displays 1 if application is sanctioned or 0 if application is not sanctioned. | keyword |
| panw.panos.application.risk_level | Risk level associated with the application (1=lowest to 5=highest). | long |
| panw.panos.application.sub_category | The application subcategory specified in the application configuration properties. | keyword |
| panw.panos.application.technology | The application technology specified in the application configuration properties. Values are: browser-based, client-server, network-protocol, peer-to-peer. | keyword |
| panw.panos.application.tunneled | Name of the tunneled application. | keyword |
| panw.panos.area_code | Area within a Public Land Mobile Network (PLMN). | keyword |
| panw.panos.attempted_gateways | The fields that are collected for each gateway connection attempt with the gateway name, SSL response time, and priority (Each field entry is separated by commas such as g82-gateway,12,3. Each gateway entry is separated by semicolons such as g83-gateway,10,2;g84-gateway,-1,1. | keyword |
| panw.panos.auth_method | A string showing the authentication type, such as LDAP, RADIUS, or SAML. | keyword |
| panw.panos.authentication.id | Unique ID given across primary authentication and additional (multi factor) authentication. | keyword |
| panw.panos.authentication.policy | Policy invoked for authentication before allowing access to a protected resource. | keyword |
| panw.panos.authentication.protocol | Indicates the authentication protocol used by the server. For example, PEAP with GTC. | keyword |
| panw.panos.before_change_detail | This field is in custom logs only; it is not in the default format. It contains the full xpath before the configuration change. | keyword |
| panw.panos.bytes_received | Number of bytes in the server-to-client direction of the session. | long |
| panw.panos.bytes_sent | Number of bytes in the client-to-server direction of the session. | long |
| panw.panos.category | A summary of the kind of threat or harm posed to the network, user, or host. | keyword |
| panw.panos.cause_code | GTP cause value in logs responses which contain an Information Element that provides information about acceptance or rejection of GTP requests by a network node. | keyword |
| panw.panos.cell.id | Base station within an area code. | keyword |
| panw.panos.certificate.fingerprint | A hash of the certificate in x509 binary format. | keyword |
| panw.panos.certificate.flags | The certificate flags can return seven values: b_resume_session, b_cert_cn_truncated, b_issuer_cn_truncated, b_root_cn_truncated, b_sni_truncated, b_cert_type, padding3. | keyword |
| panw.panos.certificate.not_after | The time the certificate expires (certificate becomes invalid after this time). | date |
| panw.panos.certificate.not_before | The time the certificate became valid (certificate in invalid before this time). | date |
| panw.panos.certificate.raw_size | The raw certificate key size. | keyword |
| panw.panos.certificate.serial_number | The unique identifier of the certificate (generated by the certificate issuer). | keyword |
| panw.panos.certificate.size | The certificate key size. | long |
| panw.panos.certificate.version | The certificate version (V1, V2, or V3). | keyword |
| panw.panos.chain_status | Whether the chain is trusted. Values are: Uninspected, Untrusted, Trusted, Incomplete | keyword |
| panw.panos.client.os | The client device’s OS type (for example, Windows or Linux). | keyword |
| panw.panos.client.os_version | The client device’s OS version. | keyword |
| panw.panos.client_type | Type of client to used by administrator or complete authentication. | keyword |
| panw.panos.client_ver | The client's GlobalProtect app version. | keyword |
| panw.panos.cloud_report.id | Unique 32 character ID for a file scanned by the DLP cloud service sent by a firewall. | keyword |
| panw.panos.cmd | Command performed by the Admin; values are add, clone, commit, delete, edit, move, rename, set, or any command generated by cli, gui, gui-opt, gnmi, or rest; | keyword |
| panw.panos.cmd_source | Source of the command that generated the audit log. Value are: cli, gui, gui-op, gnmi, rest. | keyword |
| panw.panos.comment | The audit comment entered in a policy rule configuration change. | keyword |
| panw.panos.config_version | The software version. | keyword |
| panw.panos.connect_method | A string showing the how the GlobalProtect app connects to Gateway, (for example, on-demand or user-logon. | keyword |
| panw.panos.container.id | The container ID of the PAN-NGFW pod on the Kubernetes node where the application POD is deployed. | keyword |
| panw.panos.content_version | Applications and Threats version on your firewall when the log was generated. | keyword |
| panw.panos.datasource | Source from which mapping information is collected. | keyword |
| panw.panos.datasource_subtype | The mechanism used to identify the IP address-to-username mappings within a data source. | keyword |
| panw.panos.datasource_type | The source from which mapping information is collected. | keyword |
| panw.panos.datasourcename | User-ID source that sends the IP (Port)-User Mapping. | keyword |
| panw.panos.datasourcetype | Mechanism used to identify the IP/User mappings within a data source. | keyword |
| panw.panos.description | Additional information for any event that has occurred. | text |
| panw.panos.destination.ip | Original session destination IP address. | ip |
| panw.panos.destination.location | Destination country or Internal region for private addresses. Maximum length is 32 bytes. | keyword |
| panw.panos.destination.nat.ip | If destination NAT performed, the post-NAT destination IP address. | ip |
| panw.panos.destination.nat.port | Post-NAT destination port. | long |
| panw.panos.destination.port | Destination port utilized by the session. | long |
| panw.panos.destination.user | Username of the user to which the session was destined. | keyword |
| panw.panos.destination.zone | Zone the session was destined to. | keyword |
| panw.panos.destination_vm_uuid | Identifies the destination universal unique identifier for a guest virtual machine in the VMware NSX environment. | keyword |
| panw.panos.device_group_hierarchy1 | A sequence of identification numbers that indicate the device group’s location within a device group hierarchy. The firewall (or virtual system) generating the log includes the identification number of each ancestor in its device group hierarchy. The shared device group (level 0) is not included in this structure. | keyword |
| panw.panos.device_group_hierarchy2 | A sequence of identification numbers that indicate the device group’s location within a device group hierarchy. The firewall (or virtual system) generating the log includes the identification number of each ancestor in its device group hierarchy. The shared device group (level 0) is not included in this structure. | keyword |
| panw.panos.device_group_hierarchy3 | A sequence of identification numbers that indicate the device group’s location within a device group hierarchy. The firewall (or virtual system) generating the log includes the identification number of each ancestor in its device group hierarchy. The shared device group (level 0) is not included in this structure. | keyword |
| panw.panos.device_group_hierarchy4 | A sequence of identification numbers that indicate the device group’s location within a device group hierarchy. The firewall (or virtual system) generating the log includes the identification number of each ancestor in its device group hierarchy. The shared device group (level 0) is not included in this structure. | keyword |
| panw.panos.device_group_id | The device group the firewall belongs to if managed by a Panorama™ management server. | keyword |
| panw.panos.device_name | The hostname of the firewall on which the session was logged. | keyword |
| panw.panos.diameter_app_id | The diameter application in the data chunk which triggered the event. Diameter Application ID is assigned by Internet Assigned Numbers Authority (IANA). | keyword |
| panw.panos.diameter_avp_code | The diameter AVP code in the data chunk which triggered the event. | keyword |
| panw.panos.diameter_cmd_code | The diameter command code in the data chunk which triggered the event. Diameter Command Code is assigned by Internet Assigned Numbers Authority (IANA). | keyword |
| panw.panos.domain_edl | The name of the external dynamic list that contains the domain name of the traffic. | keyword |
| panw.panos.dst.category | The category for the device that Device-ID identifies as the destination for the traffic. | keyword |
| panw.panos.dst.dynamic_address_group | Original destination source dynamic address group. | keyword |
| panw.panos.dst.external_dynamic_list | The name of the external dynamic list that contains the destination IP address of the traffic. | keyword |
| panw.panos.dst.host | The hostname of the device that Device-ID identifies as the destination for the traffic. | keyword |
| panw.panos.dst.mac | The MAC address for the device that Device-ID identifies as the destination for the traffic. | keyword |
| panw.panos.dst.model | The model of the device that Device-ID identifies as the destination for the traffic. | keyword |
| panw.panos.dst.os.family | The operating system type for the device that Device-ID identifies as the destination for the traffic. | keyword |
| panw.panos.dst.os.version | The version of the operating system for the device that Device-ID identifies as the destination for the traffic. | keyword |
| panw.panos.dst.profile | The device profile for the device that Device-ID identifies as the destination for the traffic. | keyword |
| panw.panos.dst.vendor | The vendor of the device that Device-ID identifies as the destination for the traffic. | keyword |
| panw.panos.dynamic_user.group.name | Name of the dynamic user group that contains the user who initiated the session. | keyword |
| panw.panos.elapsed_time | Elapsed time of the session. | long |
| panw.panos.elliptic_curve | The elliptic cryptography curve that the client and server negotiate and use for connections that use ECDHE cipher suites. | keyword |
| panw.panos.end_ip_address | IP address of a mobile subscriber allocated by a PGW/GGSN. | ip |
| panw.panos.endreason | The reason a session terminated. | keyword |
| panw.panos.error_code | An integer associated with any errors that occurred. | integer |
| panw.panos.error_message | A string showing that error that has occurred in any event. | keyword |
| panw.panos.event.id | A string showing the name of the event. | keyword |
| panw.panos.event.outcome | A string showing the outcome of the event. | keyword |
| panw.panos.event.reason | A string that shows the reason for the quarantine. | keyword |
| panw.panos.event.result | Result of the authentication attempt. | keyword |
| panw.panos.event.status | The status (success or failure) of the event. | keyword |
| panw.panos.event_code | Event code describing the GTP event. | keyword |
| panw.panos.event_type | Defines event triggered by a GTP message when checks in GTP protection profile are applied to the GTP traffic. Also triggered by the start or end of a GTP session. | keyword |
| panw.panos.evidence | A summary statement that indicates how many times the host has matched against the conditions defined in the correlation object. For example, Host visited known malware URl (19 times). | keyword |
| panw.panos.factorcompletiontime | Time the authentication was completed. | date |
| panw.panos.factorno | Indicates the use of primary authentication (1) or additional factors (2, 3). | integer |
| panw.panos.factortype | Vendor used to authenticate a user when Multi Factor authentication is present. | keyword |
| panw.panos.file.hash | Only for WildFire subtype; all other types do not use this field. The filedigest string shows the binary hash of the file sent to be analyzed by the WildFire service. | keyword |
| panw.panos.file.type | Only for WildFire subtype; all other types do not use this field. Specifies the type of file that the firewall forwarded for WildFire analysis. | keyword |
| panw.panos.flow_id | An internal numerical identifier applied to each session. | keyword |
| panw.panos.forwarded_ip | Only for the URL Filtering subtype; all other types do not use this field. The X-Forwarded-For field in the HTTP header contains the IP address of the user who requested the web page. It allows you to identify the IP address of the user, which is useful particularly if you have a proxy server on your network that replaces the user IP address with its own address in the source IP address field of the packet header. | ip |
| panw.panos.gateway | The name of the gateway that is specified on the portal configuration. | keyword |
| panw.panos.generated_time | Time the log was generated on the dataplane. | date |
| panw.panos.hash | The authentication algorithm used for the session, for example, SHA, SHA256, SHA384, etc. | keyword |
| panw.panos.high_resolution_timestamp | Time in milliseconds the log was received at the management plane. The format for this new field is YYYY-MM-DDThh:ss:sssTZD. | date |
| panw.panos.host.id | Unique ID GlobalProtect assigns to identify the host. | keyword |
| panw.panos.host.ip | Hostname or IP address of the client machine. | ip |
| panw.panos.hs_stage_c2f | The stage of the TLS handshake from the client to the firewall. | keyword |
| panw.panos.hs_stage_f2s | The stage of the TLS handshake from the firewall to the server. | keyword |
| panw.panos.http2_connection | Identifies if traffic used an HTTP/2 connection by displaying one of the following values: TCP connection session ID - session is HTTP/2, 0 - session is not HTTP/2. | keyword |
| panw.panos.http_content_type | Content type of the HTTP response data. | keyword |
| panw.panos.http_headers | Indicates the inserted HTTP header in the URL log entries on the firewall. | keyword |
| panw.panos.http_method | Describes the HTTP Method used in the web request. | keyword |
| panw.panos.imei | International Mobile Equipment Identity (IMEI) is a unique 15 or 16 digit number allocated to each mobile station equipment. | keyword |
| panw.panos.imsi | International Mobile Subscriber Identity (IMSI) is a unique number allocated to each mobile subscriber in the GSM/UMTS/EPS system. | keyword |
| panw.panos.inbound_interface | Interface that the session was sourced from. | keyword |
| panw.panos.interface | 3GPP interface from which a GTP message is received. | keyword |
| panw.panos.is_offloaded | Displays 1 if traffic flow has been offloaded or 0 if traffic flow was not offloaded. | keyword |
| panw.panos.issuer_common_name.length | The length of the issuer common name. | long |
| panw.panos.issuer_common_name.value | The name of the organization that verified the certificate’s contents. | keyword |
| panw.panos.justification | Justification for Data Filtering action. | keyword |
| panw.panos.link.change_count | Number of link flaps that occurred during the session. | long |
| panw.panos.link.switches | Contains up to four link flap entries, with each entry containing the link name, link tag, link type, physical interface, timestamp, bytes read, bytes written, link health, and link flap cause. | keyword |
| panw.panos.location | A string showing the administrator-defined location of the GlobalProtect portal or gateway. | keyword |
| panw.panos.log_profile | The MAC address of the user’s machine or device. | keyword |
| panw.panos.logged_time | The time the log was received. | date |
| panw.panos.login_duration | The length of time, in seconds, the user is connected to the GlobalProtect gateway from logging in to logging out. | long |
| panw.panos.machine.mac_address | The MAC address of the user’s machine or device. | keyword |
| panw.panos.machine.name | The name of the user’s machine. | keyword |
| panw.panos.machine.os | The operating system installed on the user’s machine or device (or on the client system). | keyword |
| panw.panos.matchname | Name of the HIP object or profile. | keyword |
| panw.panos.matchtype | Whether the document represents a HIP object or a HIP profile. | keyword |
| panw.panos.max_encapsulation | Number of packets the firewall dropped because the packet exceeded the maximum number of encapsulation levels configured in the Tunnel Inspection policy rule. | long |
| panw.panos.mcc | Mobile country code of serving core network operator. | keyword |
| panw.panos.message_type | Indicates the GTP message type. | keyword |
| panw.panos.misc | Field with variable length and containes URL,Filename based on threat sub-type. A Filename has a maximum of 63 characters. A URL has a maximum of 1023 characters. | keyword |
| panw.panos.mnc | Mobile network code of serving core network operator. | keyword |
| panw.panos.module | This field is valid only when the value of the Subtype field is general. It provides additional information about the sub-system generating the log; values are general, management, auth, ha, upgrade, chassis. | keyword |
| panw.panos.msisdn | Service identity associated with the mobile subscriber composed of a Country Code, National Destination Code and a Subscriber. Consists of decimal digits (0-9) only with a maximum of 15 digits. | keyword |
| panw.panos.network.application | Application associated with the session. | keyword |
| panw.panos.network.bytes | Number of total bytes (transmit and receive) for the session. | long |
| panw.panos.network.direction | Indicates the direction of the attack, client-to-server or server-to-client: 0 - direction of the threat is client to server, 1 - direction of the threat is server to client. | keyword |
| panw.panos.network.nat.community_id | Community ID flow-hash for the NAT 5-tuple. | keyword |
| panw.panos.network.packets | Number of total packets (transmit and receive) for the session. | long |
| panw.panos.network.pcap_id | The packet capture (pcap) ID is a 64 bit unsigned integral denoting an ID to correlate threat pcap files with extended pcaps taken as a part of that flow. All threat logs will contain either a pcap_id of 0 (no associated pcap), or an ID referencing the extended pcap file. | keyword |
| panw.panos.normalize_user | Normalized version of username being authenticated (such as appending a domain name to the username). | keyword |
| panw.panos.nsdsai_sd | The A Slice Differentiator of the Network Slice ID. | keyword |
| panw.panos.nsdsai_sst | The A Slice Service Type of the Network Slice ID. | keyword |
| panw.panos.nssai_sd | The A Slice Differentiator of the Network Slice ID. | keyword |
| panw.panos.nssai_sst | The A Slice Service Type of the Network Slice ID. | keyword |
| panw.panos.object.id | Name of the object associated with the system event. | keyword |
| panw.panos.object.name | Name of the correlation object that was matched on. | keyword |
| panw.panos.observer.serial_number | Serial number of the device that generated the log. | keyword |
| panw.panos.op_code | Identifies the operation code of application layer SS7 protocols, like MAP or CAP, in the data chunk which triggered the event. | keyword |
| panw.panos.outbound_interface | Interface that the session was destined to. | keyword |
| panw.panos.packets_received | Number of server-to-client packets for the session. | long |
| panw.panos.packets_sent | Number of client-to-server packets for the session. | long |
| panw.panos.parent_session.id | ID of the session in which this session is tunneled. Applies to inner tunnel (if two levels of tunneling) or inside content (if one level of tunneling) only. | keyword |
| panw.panos.parent_session.start_time | Year/month/day hours:minutes:seconds that the parent tunnel session began. | date |
| panw.panos.partial_hash | Machine Learning partial hash. | keyword |
| panw.panos.path | The path of the configuration command issued; up to 512 bytes in length. | keyword |
| panw.panos.payload_protocol_id | ID of the protocol for the payload in the data portion of the data chunk. | keyword |
| panw.panos.pcap_id | Unique packet capture ID that defines the location of the pcap file on the firewall. | keyword |
| panw.panos.pdu_session.id | Session ID for the collection of L4 segments inside a tunnel. | keyword |
| panw.panos.pod.name | The application POD being secured. | keyword |
| panw.panos.pod.namespace | The namespace of the application POD being secured. | keyword |
| panw.panos.policy.id | Name of the SD-WAN policy. | keyword |
| panw.panos.policy.name | The name of the Decryption policy associated with the session. | keyword |
| panw.panos.portal | The name of the GlobalProtect portal or gateway. | keyword |
| panw.panos.priority | The priority order of the gateway that is based on highest (1), high (2), medium (3), low (4), or lowest (5) to which the GlobalProtect app can connect. | keyword |
| panw.panos.private.ip | The private IP address for the user who initiated the session. | ip |
| panw.panos.private.ipv6 | The private IPv6 address for the user who initiated the session. | ip |
| panw.panos.protocol | IP protocol associated with the session. | keyword |
| panw.panos.proxy_type | The Decryption proxy type, such as Forward for Forward Proxy, Inbound for Inbound Inspection, No Decrypt for undecrypted traffic, GlobalProtect, etc. | keyword |
| panw.panos.public.ip | The public IP address for the user who initiated the session. | ip |
| panw.panos.public.ipv6 | The public IPv6 address for the user who initiated the session. | ip |
| panw.panos.radio_access_technology_type | Type of technology used for radio access. For example, EUTRAN, WLAN, Virtual, HSPA Evolution, GAN and GERAN. | keyword |
| panw.panos.reason | Reason for Data Filtering action. | keyword |
| panw.panos.received_time | Time the log was received at the management plane. | date |
| panw.panos.recipient | Specifies the name of the receiver of an email. | keyword |
| panw.panos.referrer | The Referer field in the HTTP header contains the URL of the web page that linked the user to another web page; it is the source that redirected (referred) the user to the web page that is being requested. | keyword |
| panw.panos.region | The geographical region where the traffic originates. | keyword |
| panw.panos.related_vsys | Virtual System associated with the session. | keyword |
| panw.panos.remote_user.id | IMSI identity of a remote user, and if available, one IMEI identity or one MSISDN identity. | keyword |
| panw.panos.remote_user.ip | IPv4 or IPv6 address of a remote user. | ip |
| panw.panos.repeat_count | Number of sessions with same Source IP, Destination IP, Application, and Subtype seen within 5 seconds. | long |
| panw.panos.response_time | The SSL response time of the selected gateway that is measured in milliseconds on the endpoint during tunnel setup. | long |
| panw.panos.result | Result of the configuration action; values are Submitted, Succeeded, Failed, and Unauthorized. | keyword |
| panw.panos.root_certificate_status | The status of the root certificate, for example, trusted, untrusted, or uninspected. | keyword |
| panw.panos.root_common_name.length | The length of the root common name. | long |
| panw.panos.root_common_name.value | The name of the root certificate authority. | keyword |
| panw.panos.rule_uuid | The UUID that permanently identifies the rule. | keyword |
| panw.panos.ruleset | Name of the rule that the session matched. | keyword |
| panw.panos.sccp.calling_gt | The Signaling Connection Control Part (SCCP) calling party global title (GT) in the data chunk which triggered the event. | keyword |
| panw.panos.sccp.calling_ssn | The Signaling Connection Control Part (SCCP) calling party subsystem number (SSN) in the data chunk which triggered the event. | keyword |
| panw.panos.sctp.assoc_end_reason | reason an association was terminated. | keyword |
| panw.panos.sctp.assoc_id | Number that identifies all connections for an association between two SCTP endpoints. | keyword |
| panw.panos.sctp.cause_code | Sent by an endpoint to specify reason for an error condition to other endpoint of same SCTP association. | keyword |
| panw.panos.sctp.chunk_type | Describes the type of information contained in a chunk, such as control or data. | keyword |
| panw.panos.sctp.chunks | Sum of SCTP chunks sent and received for an association. | long |
| panw.panos.sctp.chunks_received | Number of SCTP chunks received for an association. | long |
| panw.panos.sctp.chunks_sent | Number of SCTP chunks sent for an association. | long |
| panw.panos.sctp.filter | Name of the filter that the SCTP chunk matched. | keyword |
| panw.panos.sctp.stream_id | ID of the stream which carries the data chunk which triggered the event. | keyword |
| panw.panos.sctp.verification.tag_1 | Used by endpoint1 which initiates the association to verify if the SCTP packet received belongs to current SCTP association and validate the endpoint2. | keyword |
| panw.panos.sctp.verification.tag_2 | Used by endpoint2 to verify if the SCTP packet received belongs to current SCTP association and validate the endpoint1. | keyword |
| panw.panos.sdwan.cluster.name | Name of the SD-WAN cluster. | keyword |
| panw.panos.sdwan.cluster.type | Type of cluster (mesh or hub-spoke). | keyword |
| panw.panos.sdwan.device_type | Type of device (hub or branch). | keyword |
| panw.panos.sdwan.site | Name of the SD-WAN site. | keyword |
| panw.panos.selection_type | The connection method that is selected to connect to the gateway. | keyword |
| panw.panos.sender | Specifies the name of the sender of an email. | keyword |
| panw.panos.sequence_number | A 64-bit log entry identifier incremented sequentially; each log type has unique number space. | keyword |
| panw.panos.serial_number | Serial number of the user’s machine or device. | keyword |
| panw.panos.server_name_indication.length | The length of the Server Name Indication (hostname). | long |
| panw.panos.server_name_indication.value | The hostname of the server that the client is trying to contact. Using SNIs enables a server to host multiple websites and present multiple certificates on the same IP address and TCP port because each website has a unique SNI. | keyword |
| panw.panos.server_profile | Authentication server used for authentication. | keyword |
| panw.panos.session.owner | The original high availability (HA) peer session owner in an HA cluster from which the session table data was synchronized upon HA failover. | keyword |
| panw.panos.sessions.closed | Number of completed/closed sessions created. | long |
| panw.panos.sessions.created | Number of inner sessions created. | long |
| panw.panos.severity | Severity associated with the event; values are informational, low, medium, high, critical. | keyword |
| panw.panos.source.ip | Original session source IP address. | ip |
| panw.panos.source.ipv6 | IPv6 address of the user’s machine or device. | ip |
| panw.panos.source.location | Source country or Internal region for private addresses; maximum length is 32 bytes. | keyword |
| panw.panos.source.nat.ip | If Source NAT performed, the post-NAT Source IP address. | ip |
| panw.panos.source.nat.port | Post-NAT source port. | long |
| panw.panos.source.port | Source port utilized by the session. | long |
| panw.panos.source.region | The region for the user who initiated the session. | keyword |
| panw.panos.source.user | The username of the user who initiated the session. | keyword |
| panw.panos.source.zone | Zone the session was sourced from. | keyword |
| panw.panos.source_vm_uuid | Identifies the source universal unique identifier for a guest virtual machine in the VMware NSX environment. | keyword |
| panw.panos.src.category | The category for the device that Device-ID identifies as the source of the traffic. | keyword |
| panw.panos.src.dynamic_address_group | Original session source dynamic address group. | keyword |
| panw.panos.src.external_dynamic_list | The name of the external dynamic list that contains the source IP address of the traffic. | keyword |
| panw.panos.src.host | The hostname of the device that Device-ID identifies as the source of the traffic. | keyword |
| panw.panos.src.mac | The MAC address for the device that Device-ID identifies as the source of the traffic. | keyword |
| panw.panos.src.model | The model of the device that Device-ID identifies as the source of the traffic. | keyword |
| panw.panos.src.os.family | The operating system type for the device that Device-ID identifies as the source of the traffic. | keyword |
| panw.panos.src.os.version | The version of the operating system for the device that Device-ID identifies as the source of the traffic. | keyword |
| panw.panos.src.profile | The device profile for the device that Device-ID identifies as the source of the traffic. | keyword |
| panw.panos.src.vendor | The vendor of the device that Device-ID identifies as the source of the traffic. | keyword |
| panw.panos.stage | A string showing the stage of the connection (for example, before-login, login, or tunnel). | keyword |
| panw.panos.start_time | Time of session start. | date |
| panw.panos.strict_check | Number of packets the firewall dropped because the tunnel protocol header in the packet failed to comply with the RFC for the tunnel protocol, as enabled in the Tunnel Inspection policy rule (Drop packet if tunnel protocol fails strict header check). | long |
| panw.panos.sub_type | Subtype of logs. | keyword |
| panw.panos.subject | Specifies the subject of an email. | keyword |
| panw.panos.subject_common_name.length | The length of the subject common name. | long |
| panw.panos.subject_common_name.value | The domain name (the name of the server that the certificate protects). | keyword |
| panw.panos.tag.name | The tag mapped to the source IP address. | keyword |
| panw.panos.threat.id | Palo Alto Networks identifier for the threat. | keyword |
| panw.panos.threat.name | Identifier for known and custom threats. | keyword |
| panw.panos.threat_category | Describes threat categories used to classify different types of threat signatures.If a domain external dynamic list generated the log, domain-edl populates this field. | keyword |
| panw.panos.timeout | Timeout after which the IP/User Mappings and IP address-to-tag mapping are cleared. | integer |
| panw.panos.tls.auth | The authentication algorithm used for the session, for example, SHA, SHA256, SHA384, etc. | keyword |
| panw.panos.tls.encryption | The algorithm used to encrypt the session data, such as AES-128-CBC, AES-256-GCM, etc. | keyword |
| panw.panos.tls.error_type | The type of error that occurred: Cipher, Resource, Resume, Version, Protocol, Certificate, Feature, or HSM. | keyword |
| panw.panos.tls.key_exchange_algorithm | The key exchange algorithm used for the session. | keyword |
| panw.panos.tls.version | The version of TLS protocol used for the session. | keyword |
| panw.panos.tunnel_endpoint.identifier1 | Identifies the GTP tunnel in the network node. TEID1 is the first TEID in the GTP message. | keyword |
| panw.panos.tunnel_endpoint.identifier2 | Identifies the GTP tunnel in the network node. TEID2 is the second TEID in the GTP message. | keyword |
| panw.panos.tunnel_fragment | Number of packets the firewall dropped because of fragmentation errors. | long |
| panw.panos.tunnel_inspection_rule | Name of the tunnel inspection rule matching the cleartext tunnel traffic. | keyword |
| panw.panos.tunnel_type | Type of tunnel, such as GRE or IPSec or SSLVPN. | keyword |
| panw.panos.type | Specifies the type of log; values are HIP-MATCH, CONFIG, GLOBALPROTECT, THREAT, TRAFFIC, USERID, AUTHENTICATION, CORRELATION, DECRYPTION, GTP, IPTAG, SCTP, SYSTEM, AUDIT. | keyword |
| panw.panos.ugflags | Displays whether the user group that was found during user group mapping. Supported values are: User Group Found—Indicates whether the user could be mapped to a group.Duplicate User—Indicates whether duplicate users were found in a user group. Displays N/A if no user group is found. | keyword |
| panw.panos.unknown_protocol | Number of packets the firewall dropped because the packet contains an unknown protocol, as enabled in the Tunnel Inspection policy rule (Drop packet if unknown protocol inside tunnel). | long |
| panw.panos.url.category | For URL Subtype, it is the URL Category; For WildFire subtype, it is the verdict on the file and is either ‘malware’, ‘phishing’, ‘grayware’, or ‘benign’; For other subtypes, the value is ‘any’. | keyword |
| panw.panos.url_category_list | Lists the URL Filtering categories that the firewall used to enforce policy. | keyword |
| panw.panos.url_idx | Used in URL Filtering and WildFire subtypes. When an application uses TCP keepalives to keep a connection open for a length of time, all the log entries for that session have a single session ID. In such cases, when you have a single threat log (and session ID) that includes multiple URL entries, the url_idx is a counter that allows you to correlate the order of each log entry within the single session. For example, to learn the URL of a file that the firewall forwarded to WildFire for analysis, locate the session ID and the url_idx from the WildFire Submissions log and search for the same session ID and url_idx in your URL filtering logs. The log entry that matches the session ID and url_idx will contain the URL of the file that was forwarded to WildFire. | keyword |
| panw.panos.user | End user being authenticated. | keyword |
| panw.panos.user_agent | Only for the URL Filtering subtype; all other types do not use this field. The User Agent field specifies the web browser that the user used to access the URL, for example Internet Explorer. This information is sent in the HTTP request to the server. | keyword |
| panw.panos.user_by_source | Indicates the username received from the source through IP address-to-username mapping. | keyword |
| panw.panos.vendor | Vendor providing additional factor authentication. | keyword |
| panw.panos.virtual_sys | Virtual System associated with the session. | keyword |
| panw.panos.vsys_id | A unique identifier for a virtual system on a Palo Alto Networks firewall. | keyword |
| panw.panos.vsys_name | The name of the virtual system associated with the session; only valid on firewalls enabled for multiple virtual systems. | keyword |
| panw.panos.wildfire.name | Only for WildFire subtype; all other types do not use this field. The cloud string displays the FQDN of either the WildFire appliance (private) or the WildFire cloud (public) from where the file was uploaded for analysis. | keyword |
| panw.panos.wildfire.report_id | Only for Data Filtering and WildFire subtype; all other types do not use this field. Identifies the analysis request on the firewall, WildFire cloud, or the WildFire appliance. | keyword |
| panw.panos.x_forwarded_for | Only for the URL Filtering subtype; all other types do not use this field. The X-Forwarded-For field in the HTTP header contains the IP address of the user who requested the web page. It allows you to identify the IP address of the user, which is useful particularly if you have a proxy server on your network that replaces the user IP address with its own address in the source IP address field of the packet header. | keyword |
| panw.panos.xff.ip | The IP address of the user who requested the web page or the IP address of the next to last device that the request traversed. If the request goes through one or more proxies, load balancers, or other upstream devices, the firewall displays the IP address of the most recent device. | ip |
| related.hash | All the hashes seen on your event. Populating this field, then using it to search for hashes can help in situations where you're unsure what the hash algorithm is (and therefore which key name to search). | keyword |
| related.hosts | All hostnames or other host identifiers seen on your event. Example identifiers include FQDNs, domain names, workstation names, or aliases. | keyword |
| related.ip | All of the IPs seen on your event. | ip |
| related.user | All the user names or other user identifiers seen on the event. | keyword |
| rule.name | The name of the rule or signature generating the event. | keyword |
| rule.uuid | A rule ID that is unique within the scope of a set or group of agents, observers, or other entities using the rule for detection of this event. | keyword |
| server.bytes | Bytes sent from the server to the client. | long |
| server.ip | IP address of the server (IPv4 or IPv6). | ip |
| server.nat.ip | Translated ip of destination based NAT sessions (e.g. internet to private DMZ) Typically used with load balancers, firewalls, or routers. | ip |
| server.nat.port | Translated port of destination based NAT sessions (e.g. internet to private DMZ) Typically used with load balancers, firewalls, or routers. | long |
| server.packets | Packets sent from the server to the client. | long |
| server.port | Port of the server. | long |
| server.user.name | Short name or login of the user. | keyword |
| server.user.name.text | Multi-field of `server.user.name`. | match_only_text |
| session.start_time | Time of session start. | date |
| source.address | Some event source addresses are defined ambiguously. The event will sometimes list an IP, a domain or a unix socket.  You should always store the raw address in the `.address` field. Then it should be duplicated to `.ip` or `.domain`, depending on which one it is. | keyword |
| source.as.number | Unique number allocated to the autonomous system. The autonomous system number (ASN) uniquely identifies each network on the Internet. | long |
| source.as.organization.name | Organization name. | keyword |
| source.as.organization.name.text | Multi-field of `source.as.organization.name`. | match_only_text |
| source.bytes | Bytes sent from the source to the destination. | long |
| source.geo.city_name | City name. | keyword |
| source.geo.continent_name | Name of the continent. | keyword |
| source.geo.country_iso_code | Country ISO code. | keyword |
| source.geo.country_name | Country name. | keyword |
| source.geo.location | Longitude and latitude. | geo_point |
| source.geo.name | User-defined description of a location, at the level of granularity they care about. Could be the name of their data centers, the floor number, if this describes a local physical entity, city names. Not typically used in automated geolocation. | keyword |
| source.geo.region_iso_code | Region ISO code. | keyword |
| source.geo.region_name | Region name. | keyword |
| source.ip | IP address of the source (IPv4 or IPv6). | ip |
| source.nat.ip | Translated ip of source based NAT sessions (e.g. internal client to internet) Typically connections traversing load balancers, firewalls, or routers. | ip |
| source.nat.port | Translated port of source based NAT sessions. (e.g. internal client to internet) Typically used with load balancers, firewalls, or routers. | long |
| source.packets | Packets sent from the source to the destination. | long |
| source.port | Port of the source. | long |
| source.user.domain | Name of the directory the user is a member of. For example, an LDAP or Active Directory domain name. | keyword |
| source.user.email | User email address. | keyword |
| source.user.name | Short name or login of the user. | keyword |
| source.user.name.text | Multi-field of `source.user.name`. | match_only_text |
| syslog.facility | Syslog numeric facility of the event. | long |
| syslog.facility_label | Syslog text-based facility of the event. | keyword |
| syslog.priority | Syslog priority of the event. | long |
| syslog.severity_label | Syslog text-based severity of the event. | keyword |
| tags | List of keywords used to tag each event. | keyword |
| tls.cipher | String indicating the cipher used during the current connection. | keyword |
| tls.client.hash.md5 | Certificate fingerprint using the MD5 digest of DER-encoded version of certificate offered by the client. For consistency with other hash values, this value should be formatted as an uppercase hash. | keyword |
| tls.client.hash.sha1 | Certificate fingerprint using the SHA1 digest of DER-encoded version of certificate offered by the client. For consistency with other hash values, this value should be formatted as an uppercase hash. | keyword |
| tls.client.hash.sha256 | Certificate fingerprint using the SHA256 digest of DER-encoded version of certificate offered by the client. For consistency with other hash values, this value should be formatted as an uppercase hash. | keyword |
| tls.client.not_after | Date/Time indicating when client certificate is no longer considered valid. | date |
| tls.client.not_before | Date/Time indicating when client certificate is first considered valid. | date |
| tls.client.server_name | Also called an SNI, this tells the server which hostname to which the client is attempting to connect to. When this value is available, it should get copied to `destination.domain`. | keyword |
| tls.client.x509.issuer.common_name | List of common name (CN) of issuing certificate authority. | keyword |
| tls.client.x509.public_key_size | The size of the public key space in bits. | long |
| tls.client.x509.serial_number | Unique serial number issued by the certificate authority. For consistency, this should be encoded in base 16 and formatted without colons and uppercase characters. | keyword |
| tls.client.x509.subject.common_name | List of common names (CN) of subject. | keyword |
| tls.client.x509.version_number | Version of x509 format. | keyword |
| tls.curve | String indicating the curve used for the given cipher, when applicable. | keyword |
| tls.version | Numeric part of the version parsed from the original string. | keyword |
| tls.version_protocol | Normalized lowercase protocol name parsed from original string. | keyword |
| url.domain | Domain of the url, such as "www.elastic.co". In some cases a URL may refer to an IP and/or port directly, without a domain name. In this case, the IP address would go to the `domain` field. If the URL contains a literal IPv6 address enclosed by `[` and `]` (IETF RFC 2732), the `[` and `]` characters should also be captured in the `domain` field. | keyword |
| url.extension | The field contains the file extension from the original request url, excluding the leading dot. The file extension is only set if it exists, as not every url has a file extension. The leading period must not be included. For example, the value must be "png", not ".png". Note that when the file name has multiple extensions (example.tar.gz), only the last one should be captured ("gz", not "tar.gz"). | keyword |
| url.original | Unmodified original url as seen in the event source. Note that in network monitoring, the observed URL may be a full URL, whereas in access logs, the URL is often just represented as a path. This field is meant to represent the URL as it was observed, complete or not. | wildcard |
| url.original.text | Multi-field of `url.original`. | match_only_text |
| url.path | Path of the request, such as "/search". | wildcard |
| url.port | Port of the request, such as 443. | long |
| url.query | The query field describes the query string of the request, such as "q=elasticsearch". The `?` is excluded from the query string. If a URL contains no `?`, there is no query field. If there is a `?` but no query, the query field exists with an empty string. The `exists` query can be used to differentiate between the two cases. | keyword |
| user.domain | Name of the directory the user is a member of. For example, an LDAP or Active Directory domain name. | keyword |
| user.email | User email address. | keyword |
| user.name | Short name or login of the user. | keyword |
| user.name.text | Multi-field of `user.name`. | match_only_text |
| user_agent.device.name | Name of the device. | keyword |
| user_agent.name | Name of the user agent. | keyword |
| user_agent.original | Unparsed user_agent string. | keyword |
| user_agent.original.text | Multi-field of `user_agent.original`. | match_only_text |
| user_agent.os.full | Operating system name, including the version or code name. | keyword |
| user_agent.os.full.text | Multi-field of `user_agent.os.full`. | match_only_text |
| user_agent.os.name | Operating system name, without the version. | keyword |
| user_agent.os.name.text | Multi-field of `user_agent.os.name`. | match_only_text |
| user_agent.os.version | Operating system version as a raw string. | keyword |
| user_agent.version | Version of the user agent. | keyword |


### Example event

An example event for `panos` looks as following:

```json
{
    "@timestamp": "2012-10-30T09:46:12.000Z",
    "agent": {
        "ephemeral_id": "df9cb56b-dbbb-4b0c-919d-cfab75836e80",
        "id": "01cab955-0bdd-4b67-97d1-743fd31e19ea",
        "name": "docker-fleet-agent",
        "type": "filebeat",
        "version": "8.14.1"
    },
    "data_stream": {
        "dataset": "panw.panos",
        "namespace": "65288",
        "type": "logs"
    },
    "destination": {
        "domain": "lorexx.cn",
        "geo": {
            "city_name": "Changchun",
            "continent_name": "Asia",
            "country_iso_code": "CN",
            "country_name": "China",
            "location": {
                "lat": 43.88,
                "lon": 125.3228
            },
            "name": "United States",
            "region_iso_code": "CN-22",
            "region_name": "Jilin Sheng"
        },
        "ip": "175.16.199.1",
        "port": 80
    },
    "ecs": {
        "version": "8.17.0"
    },
    "elastic_agent": {
        "id": "01cab955-0bdd-4b67-97d1-743fd31e19ea",
        "snapshot": false,
        "version": "8.14.1"
    },
    "event": {
        "action": "url_filtering",
        "agent_id_status": "verified",
        "category": [
            "intrusion_detection",
            "threat",
            "network"
        ],
        "created": "2024-08-15T19:31:00.703Z",
        "dataset": "panw.panos",
        "ingested": "2024-08-15T19:31:10Z",
        "kind": "alert",
        "original": "<14>Nov 30 16:09:08 PA-220 1,2012/10/30 09:46:12,01606001116,THREAT,url,1,2012/04/10 04:39:56,192.168.0.2,175.16.199.1,0.0.0.0,0.0.0.0,rule1,crusher,,web-browsing,vsys1,trust,untrust,ethernet1/2,ethernet1/1,forwardAll,2012/04/10 04:39:58,25149,1,59309,80,0,0,0x208000,tcp,alert,\"lorexx.cn/loader.exe\",(9999),not-resolved,informational,client-to-server,0,0x0,192.168.0.0-192.168.255.255,United States,0,text/html",
        "outcome": "success",
        "severity": 5,
        "timezone": "+00:00",
        "type": [
            "allowed"
        ]
    },
    "input": {
        "type": "tcp"
    },
    "labels": {
        "captive_portal": true,
        "container_page": true
    },
    "log": {
        "level": "informational",
        "source": {
            "address": "172.19.0.4:52222"
        },
        "syslog": {
            "facility": {
                "code": 1,
                "name": "user-level"
            },
            "hostname": "PA-220",
            "priority": 14,
            "severity": {
                "code": 6,
                "name": "Informational"
            }
        }
    },
    "message": "192.168.0.2,175.16.199.1,0.0.0.0,0.0.0.0,rule1,crusher,,web-browsing,vsys1,trust,untrust,ethernet1/2,ethernet1/1,forwardAll,2012/04/10 04:39:58,25149,1,59309,80,0,0,0x208000,tcp,alert,\"lorexx.cn/loader.exe\",(9999),not-resolved,informational,client-to-server,0,0x0,192.168.0.0-192.168.255.255,United States,0,text/html",
    "network": {
        "application": "web-browsing",
        "community_id": "1:CpnxxiYk2GolQXL1AiyOIq2jeIE=",
        "direction": "inbound",
        "transport": "tcp",
        "type": "ipv4"
    },
    "observer": {
        "egress": {
            "interface": {
                "name": "ethernet1/1"
            },
            "zone": "untrust"
        },
        "ingress": {
            "interface": {
                "name": "ethernet1/2"
            },
            "zone": "trust"
        },
        "product": "PAN-OS",
        "serial_number": "01606001116",
        "type": "firewall",
        "vendor": "Palo Alto Networks"
    },
    "panw": {
        "panos": {
            "action": "alert",
            "action_flags": "0x0",
            "flow_id": "25149",
            "generated_time": "2012-04-10T04:39:56.000Z",
            "http_content_type": "text/html",
            "log_profile": "forwardAll",
            "logged_time": "2012-04-10T04:39:58.000Z",
            "received_time": "2012-10-30T09:46:12.000Z",
            "repeat_count": 1,
            "ruleset": "rule1",
            "sequence_number": "0",
            "sub_type": "url",
            "threat": {
                "id": "9999",
                "name": "URL-filtering"
            },
            "type": "THREAT",
            "url": {
                "category": "not-resolved"
            },
            "virtual_sys": "vsys1"
        }
    },
    "related": {
        "ip": [
            "192.168.0.2",
            "175.16.199.1"
        ],
        "user": [
            "crusher"
        ]
    },
    "rule": {
        "name": "rule1"
    },
    "source": {
        "geo": {
            "name": "192.168.0.0-192.168.255.255"
        },
        "ip": "192.168.0.2",
        "port": 59309,
        "user": {
            "name": "crusher"
        }
    },
    "tags": [
        "preserve_original_event",
        "panw-panos",
        "forwarded"
    ],
    "url": {
        "domain": "lorexx.cn",
        "extension": "exe",
        "original": "lorexx.cn/loader.exe",
        "path": "/loader.exe"
    },
    "user": {
        "name": "crusher"
    }
}
```

### Inputs

These inputs can be used in this integration:

* [tcp](https://www.elastic.co/docs/reference/integrations/tcp)
* [udp](https://www.elastic.co/docs/reference/integrations/udp)
* [logfile](https://www.elastic.co/docs/reference/integrations/filestream)
