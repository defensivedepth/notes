# Zeek `http.log` Field Coverage: ECS vs OCSF

This table compares how well the Elastic Common Schema (ECS) and the Open Cybersecurity Schema Framework (OCSF) cover the fields found in Zeek's `http.log`.

| **Zeek Field**              | **Description**                                          | **ECS Mapping**                     | **OCSF Mapping**               | **Coverage Notes**                      |
|----------------------------|----------------------------------------------------------|-------------------------------------|--------------------------------|-----------------------------------------|
| `ts`                       | Timestamp of the HTTP transaction                        | `event.start`                       | `start_time`                   | ✅ Both cover                           |
| `uid`                      | Unique connection ID                                     | `http.request.id`                  | `session_id`                   | ✅ Both cover                           |
| `id.orig_h` / `id.orig_p`  | Source IP and port                                       | `source.ip`, `source.port`         | `src_endpoint.ip`, `.port`     | ✅ Both cover                           |
| `id.resp_h` / `id.resp_p`  | Destination IP and port                                  | `destination.ip`, `destination.port` | `dst_endpoint.ip`, `.port`     | ✅ Both cover                           |
| `method`                   | HTTP method (GET, POST, etc.)                            | `http.request.method`              | `method`                       | ✅ Both cover                           |
| `host`                     | HTTP Host header                                         | `host.hostname`                    | `host`                         | ✅ Both cover                           |
| `uri`                      | URI path and query                                       | `url.full`, `url.path`, `url.query`| `url`                          | ✅ Both cover (ECS more detailed)       |
| `referrer`                 | Referrer header                                          | `http.request.referrer`            | `referrer_url`                 | ✅ Both cover                           |
| `user_agent`               | User-Agent header                                        | `user_agent.original`              | `user_agent`                   | ✅ Both cover                           |
| `version`                  | HTTP version (e.g., 1.1)                                 | `http.version`                     | `http_version`                 | ✅ Both cover                           |
| `status_code`              | HTTP response code (200, 404, etc.)                      | `http.response.status_code`        | `http_status_code`             | ✅ Both cover                           |
| `status_msg`               | HTTP status message (OK, Not Found, etc.)                | `zeek.http.status_msg`             | `response_message`             | ✅ Both cover                           |
| `originating_bytes`        | Bytes sent from client                                   | `http.request.body.bytes`          | `bytes_out`                    | ✅ Both cover                           |
| `response_body_len`        | Bytes sent from server                                   | `http.response.body.bytes`         | `bytes_in`                     | ✅ Both cover                           |
| `tags`                     | Zeek-specific tags (e.g., truncated headers)             | `zeek.http.tags`                   | ❌ Not covered                  | ❌ OCSF lacks support                   |
| `info_code` / `info_msg`   | Rare response info details                               | `zeek.http.info_code`, `info_msg`  | ❌ Not covered                  | ❌ Only ECS Zeek extension covers       |

## Summary

- ✅ **ECS** covers **all** fields, especially when using the `zeek.*` namespace.
- ⚠️ **OCSF** covers **core HTTP fields well** but lacks support for **Zeek-specific tags and diagnostic fields**.
