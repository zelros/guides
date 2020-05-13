<img src="./imgs/zelros.svg" width="250">

# Checklist

<table>

<tr>
<th>Check</th>
<th>Yes/No</th>
</tr>

<tr><td colspan="2"><b>100: Think as Business API</b></td></tr>

<tr><td>API has only 1 endpoint</td><td></td></tr>

<tr><td colspan="2"><b>200: Stick to the standard</b></td></tr>

<tr><td colspan="2"><i>210: Start with Swagger</i></td></tr>
<tr><td>API has only one YAML swagger file in the source code</td><td></td></tr>
<tr><td>API serve a JSON swagger file at root</td><td></td></tr>
<tr><td>All E2E tests use a generated SDK based on YAML file to access to the API</td><td></td></tr>

<tr><td colspan="2"><i>220: Fit to the basics</i></td></tr>

<tr><td colspan="2"><i>221: Requests</i></td></tr>
<tr><td>API has only business routes</td><td></td></tr>
<tr><td>All actions have only 1 route + 1 verb</td><td></td></tr>
<tr><td>All links have a maximum depth of 2 in path</td><td></td></tr>
<tr><td>All resource have plural names in paths</td><td></td></tr>
<tr><td>Paths are only lowercase</td><td></td></tr>
<tr><td>All JSON keys are in lowercase</td><td></td></tr>
<tr><td>All paths are in snake_case</td><td></td></tr>
<tr><td>All JSON keys are in snake_case</td><td></td></tr>
<tr><td>All request payloads are JSON format</td><td></td></tr>
<tr><td>All ressources are identified by a UUID</td><td></td></tr>
<tr><td>All options are querystring parameters</td><td></td></tr>
<tr><td>Versions are managed in the Accepts header</td><td></td></tr>

<tr><td colspan="2"><i>222: Responses</i></td></tr>
<tr><td>Response payload are JSON</td><td></td></tr>
<tr><td>Timestamps are UTC times formatted in ISO88601</td><td></td></tr>
<tr><td>Type of answer uses standard HTTP status code</td><td></td></tr>
<tr><td>Nested foreign key relations are full record or foreign keys</td><td></td></tr>
<tr><td>All routes return a generated Request-Id header (UUID format)</td><td></td></tr>
<tr><td>All routes accept a Session-Id header and return it in the response (UUID format)</td><td></td></tr>
<tr><td>Errors are structured with an documented ID</td><td></td></tr>
<tr><td>Errors ID are documented</td><td></td></tr>
<tr><td>Type of error uses standard HTTP status code</td><td></td></tr>

<tr><td colspan="2"><i>230: Usual cases</i></td></tr>
<tr><td>Search routes are normalized</td><td></td></tr>
<tr><td>Links are normalized</td><td></td></tr>
<tr><td>Action routes are normalized</td><td></td></tr>
<tr><td>CRUD routes are normalized</td><td></td></tr>

</table>
