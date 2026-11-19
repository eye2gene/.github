# Security Policy

This is the org-wide default security policy for Eye2Gene repositories. If a
repository defines its own `SECURITY.md`, that takes precedence.

## Reporting a Vulnerability

Please do **not** put the details of a security vulnerability in a public
GitHub issue.

Instead, report it privately:

- **Public repositories:** open the repository's **Security** tab and choose
  **Report a vulnerability**, or go straight to
  `https://github.com/eye2gene/<repository>/security/advisories/new`. Private
  vulnerability reporting is enabled on Eye2Gene's public repositories, and
  only the repository's maintainers see the report.
- **If that option is missing or does not work for you:** open an issue in the
  repository that says only that you have a security report to make, with no
  detail of the vulnerability. A maintainer will reply with a private way to
  send it.
- **Eye2Gene staff and collaborators, for any repository:** tell the
  Information Security Officer directly.

Include:

- A description of the vulnerability and its potential impact
- Steps to reproduce
- Affected repository/version/commit, if known

Reports are triaged and remediated by Engineering under POL-8 Threat and
Vulnerability Management Policy. We aim to acknowledge external reports within
5 business days; that target is a courtesy to reporters, not a QMS commitment.

## Scope

This covers vulnerabilities in Eye2Gene-maintained code. For issues in
third-party dependencies (SOUP), report upstream as well as to us if it
affects an Eye2Gene product — see the repository's SOUP/dependency list if it
maintains one.

## Security Incidents and Patient Data

A report that indicates an actual or suspected security incident (unauthorised
access, altered data, compromised credentials, misuse of Eye2Gene servers) or
any exposure of patient or personal data is handled under SOP-33 Security
Incident Response Plan, in addition to the vulnerability handling above:

- Notify the VP of Engineering and the Information Security Officer
  immediately. For a suspected personal-data breach, also notify the Data
  Protection Officer (SOP-33 §5.2).
- Log the event as a Quality Event of type Data Breach / Security Incident in
  the QMS repository (SOP-33 §5.2.4) so it is tracked to closure and root-cause
  analysis (SOP-33 §5.9).
