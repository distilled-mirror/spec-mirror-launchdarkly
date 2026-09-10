> For clean Markdown of any page, append .md to the page URL.
> For a complete documentation index, see https://launchdarkly.com/docs/llms.txt.
> For AI client integration (Claude Code, Cursor, etc.), connect to the MCP server at https://launchdarkly.com/docs/_mcp/server.

# API access tokens

> This topic explains how to use API access tokens to authenticate with the LaunchDarkly REST API.

This topic explains how to use API access tokens to authenticate with the [LaunchDarkly REST API](/api), as well as constraints and suggestions for implementing them.

#### API access tokens are private

Only you have access to the secret values of tokens you create. Other account members cannot access them. Administrators can delete your tokens, but cannot view their values.

## Scope personal API access tokens

You can scope your API tokens to restrict the set of operations they can perform. For example, you can build an integration that only has read access to the REST API.

When you [create an access token](/home/account/api-create), use the **Role** menu to set the scope for your access token:

* You can select a specific role. Choosing a role gives the token the same permissions as the role.
  * Select Reader, Writer, Admin, or Owner if you want to use a base role.
  * Select **Custom** to choose another role, either one you've created or one provided by LaunchDarkly. This option is available only if your LaunchDarkly subscription includes custom roles.
* You can select **Inline policy** to create a role policy that applies only to this token. This option is available only if your LaunchDarkly subscription includes custom roles.

#### Never share an API access token

API access tokens are secrets. If you share your access token with others, they may be able to use it to impersonate you, or perform actions with it that could later be attributed to you or your integration erroneously.

You can also use the REST API: [Access tokens](/api/access-tokens)

## Access token permissions

#### Personal API access tokens and the principle of least privilege

As a best practice, we recommend giving your tokens the smallest scope required for your integration. For example, if your integration is not designed to modify your Production environment, use a custom role or inline policy to restrict access appropriately.

#### Using custom roles in access tokens

If you use custom roles to scope your access tokens, modifying the permissions of the custom roles will also modify the permissions of related tokens.

There are two types of tokens you can create in LaunchDarkly. You can create a personal token, which is linked to an account member's account, or a service token, which is independent of the account that created it.

The different token types respond differently when their creators' permissions change. Because of this, you may want to use different types of tokens for different things.

### Personal tokens

You can configure a personal access token to have the same permissions that you do, or more restrictive permissions. Your personal tokens can never do more than you can in LaunchDarkly.

If you have permissions through a custom role, you can configure a personal access token to also have that custom role. If the custom role uses [role attributes](/home/getting-started/vocabulary#role-attribute), then the access token will have the same value for the role attribute as you do. For example, suppose an administrator assigned you a custom role and set the value of its role attribute to `projectA` during the assignment. When you configure a personal access token with this custom role, the access token will also use the custom role with role attribute set to `projectA`. To learn more, read [Using role scope](/home/account/roles/role-scope).

If your own permissions are ever reduced, personal tokens you have created have reduced scope as well. For example, if you have a base role of Writer and create a Writer token, but then are downgraded to a base role of Reader, your Writer token is also downgraded. After your permissions change, that token behaves like a Reader token.

If an account member with personal access tokens is removed from your LaunchDarkly team, their personal tokens are deactivated.

Use a personal token when you want to access the LaunchDarkly API for your temporary or personal use. Personal tokens are tied to your member ID, which is an immutable identifier that does not change even if your email address is updated, for example through SCIM during an identity provider migration. This means your personal tokens remain valid after an email change.

### Service tokens

#### Service tokens are available to customers on select plans

Service tokens are only available to customers on select plans. To learn more, [read about our pricing](https://launchdarkly.com/pricing/). To upgrade your plan, [contact Sales](https://launchdarkly.com/contact-sales/).

Unlike personal tokens, service tokens are not tied to your LaunchDarkly profile. You can assign an existing role to a service token, or create a custom role for it to use.

A service token's permissions are permanently fixed after you create it. You cannot edit the permissions of a service token, and even if your permissions change, the service token's permissions stay the same.

#### Service tokens can only have the permissions of the role assigned to their creator

If you create a service token and give it more access than you have, the service token will fail to perform actions or access resources that you do not have permission to perform or access.

You can never give a service token more permissions than you have.

Use a service token to create long-term integrations with the LaunchDarkly API.

## API token usage visibility

The "Last used" column in the API access tokens list reflects only requests made to the LaunchDarkly REST API. It does not update when a token is used for other purposes, such as sending trace data or other telemetry through the OpenTelemetry Protocol (OTLP) endpoint.

If a token is used exclusively for data ingestion through these endpoints, it may display as "Never used" in the UI even though it is actively sending data to LaunchDarkly.

## Restricting who can create and manage API access tokens

By default, all account members can create access tokens limited to their existing permissions. For example, account members with a base role of Reader can only create tokens with a Reader role, whereas account members with a base role of Owner or Admin can create tokens with any permission level.

You can restrict account members from creating or managing access tokens with custom roles.

To learn more, read [Actions in custom roles](/home/account/roles/role-actions).
