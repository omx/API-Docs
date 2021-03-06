<center><a href='https://theomx.com'><img src="https://theomx.com/assets/new/omx_logo-e309ca445f44378e718aa40cd5c054c14d18337b4706f912ec3ec47935432af1.png" width="200" ></a></center>

# How It Works

The OMX API accepts queries using a two-phase approach: the authentication handshake and then the actual query itself. In either scenario, the system is queried with a version number, an endpoint (which defines the type of information requested) and a payload containing input parameters (sent by POST), and then returns a resultset.

The URL structure for querying the OMX API is as follows:

```
https://theomx.com/api/1.0/authenticate
```

"1.0" represents the version number, and "authenticate" represents the endpoint. All versions and endpoints will be documented here.

# Authentication Phase

The OMX API is queried with a key:

```
{
  api_key: ABC123
}
```

The returned data provides the input parameters which were queried, plus a resultset (in this case a session token), plus any warnings and/or errors:


```
{
  datetime: gmt_time,
  key: ABC123,
  format: json,
  result: [
    token: DEF456,
    expires_on: gmt_time
  ]
  errors: [
    [789, “Incorrect key”]
  ]
  warnings:[
    [012, “Unspecified format”]
  ]
}
```

Note that this result set is solely illustrative; a token would never be provided if there were errors, only warnings.

Tokens should expire within one hour of issue.

# Query Phase

The token provided in the authentication phase is used as an input parameter to authenticate phase two: the actual data query. The result set will look something like this:


```
{
  datetime: gmt_time,
  token: DEF456,
  expires_on: gmt_time,
  format: json,
  page: 1,
  total_pages: 14,
  result: [
  ]
  errors: [
  ]
  warnings:[
  ]
}
```

Again, this simple result set is solely illustrative; there is no need for empty key-value pairs.

Note that if the user has selected CSV as the desired output format, nested fields will simply be concatenated.

# Permissioning and Rate Limiting

Access to the API is determined by your organization's plan. Some plans will receive truncated resultsets and some robust resultsets. All resultsets will be rate limited to prevent abuse. It is strongly recommended that systems which connect with the OMX API do not do so in user real-time. A safer architecture is to download data from OMX asynchronously and store locally for better real-time performance and no redundant data retrieval.

# Versions

- [/v1.0/endpoints](v1-0/endpoints.md) * Latest

# Error Messages

<table>
  <tr>
  <td><b>Code</b></td>
  <td><b>Message</b></td>
  <td><b>Endpoints</b></td>
  </tr>
  <tr>
  <td>0001</td>
  <td>Incorrect key</td>
  <td>authenticate</td>
  </tr>
  <tr>
  <td>0002</td>
  <td>Incorrect token</td>
  <td>organizations, opportunities</td>
  </tr>
  <tr>
  <td>0003</td>
  <td>Expired token; please reauthenticate using your API key</td>
  <td>authenticate, organizations, opportunities</td>
  </tr>
  <tr>
  <td>0004</td>
  <td>The version of the API is now retired; please update your queries to resume accessing the API.</td>
  <td>authenticate, organizations, opportunities</td>
  </tr>
  <tr>
  <td>0005</td>
  <td>Daily query cap exceeded; please review documentation on connecting asynchronously to avoid exceeding limits.</td>
  <td>authenticate, organizations, opportunities</td>
  </tr>
</table>

# Warning Messages

<table>
  <tr>
  <td><b>Code</b></td>
  <td><b>Message</b></td>
  <td><b>Endpoints</b></td>
  </tr>
  <tr>
  <td>0001</td>
  <td>API version deprecated and approaching retirement; please upgrade to the latest version</td>
  <td>authenticate, organizations, opportunities</td>
  </tr>
  <tr>
  <td>0002</td>
  <td>Daily query cap imminent; please review documentation on connecting asynchronously to avoid exceeding limits.</td>
  <td>authenticate, organizations, opportunities</td>
  </tr>
  <tr>
  <td>0003</td>
  <td>No output format specified, so defaulted to JSON</td>
  <td>authenticate, organizations, opportunities</td>
  </tr>
  <tr>
  <td>0004</td>
  <td>No page number specified, so defaulted to 1</td>
  <td>organizations, opportunities</td>
  </tr>
  <tr>
  <td>0005</td>
  <td>This is a truncated resultset, so not all data has been provided for all fields. Please upgrade your account to receive the full resultset.</td>
  <td>organizations, opportunities</td>
  </tr>
  <tr>
  <td>0006</td>
  <td>Invalid input parameter received</td>
  <td>authenticate, organizations, opportunities</td>
  </tr>
</table>

# Please Contact Us

Although every effort has been made to ensure that this documentation accurately reflects the operation of the API, please advise us if you discover any discrepancies.
