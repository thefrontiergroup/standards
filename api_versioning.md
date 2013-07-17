# API Versioning

## Version Strategy

All production APIs should use [Semantic Versioning](http://semver.org/) for
generating version numbers however it should be limited to MAJOR and MINOR
releases only.

Versions should be changed under the following circumstances:
  * The format of a request to the API has change
  * The format of a response from the API has changed
  * The meaning of any data in either a request or response has changed

An API's version should be incremented sparingly and should be tied to a
collection of work. Examples include:
  * A planned feature set
  * A time period's (most likely a sprint) worth of minor tweaks

## Deprecation

All production APIs should deprecate previous major versions as soon as
possible, keeping no more than one previous major version active. As an
example, as version 3.0 is being scheduled for release version 1.x should
be scheduled for deprecation.

When a version is scheduled to be deprecated a message should be included
in the header of every API response from that version containing the
expected deprecation date. Any client's using the API should parse the
message and inform the end user that an update of the client will be required
before the expiry date.

Once the version has been deprecated, controllers/decorators/views relating to
that version should be removed from the codebase.

## Testing

When writing API controller tests the tests should be as black box as possible
passing in and validating against actual expected JSON strings to ensure the
requests and responses meet the specifications for that version.

Once a test has been written for a version of an API controller, it should be considered
immutable for the life cycle of that version to ensure any future changes are backwards
compatible. If a new test is required this should be a good indication that the API
version should be incremented.

Only once an API version has been deprecated should the tests be removed.