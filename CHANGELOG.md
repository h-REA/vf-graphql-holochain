## 0.9.0

- **Breaking:** switched to a revision-based API for referencing updates & deletions, for compatibility with eventually-consistent distributed systems
	- `revisionId` is now a mandatory field in all updateable record types. For systems which do not implement history, it is fine to return `revisionId` with the same value as `id`.
	- Added a new optional `history` module, which services needing to implement human-facing conflict resolution capabilities (eg. eventually-consistent networks) may implement. These additional query edges and response types provide a minimal-footprint API which can be used to resolve conflicts between divergent branches of the same record. See the 'bridging' schemas beginning with `history.*` as a reference.

## 0.8.5 (unreleased)

- Fixed errata in fields being required or not:
	- `Claim.triggeredBy` is now required when creating
	- `Plan.name` is now required
	- `ScenarioDefinition.name` is now correctly required when creating, but not when updating
	- `Scenario.definedAs` is no longer required
- **Breaking:** added a new field `EconomicEvent.toLocation` for managing resource location updates. `EconomicResource.currentLocation` is no longer updateable directly.
- Fix `EconomicEvent.inScopeOf` being updateable when it should not be
- Fix `RecipeFlow.recipeFlowResource` not being required when it should be
- Fix `RecipeProcess.processConformsTo` being required when it should not be
- Added `Commitment.plannedWithin`
- Fixed IDs not being mandatory in all direct-retrieval API methods
- Added a mock GraphQLClient for direct use in UI code, to complement the mock GraphQLServer
- Fixed some geolocation fields being present in the generated schema when the `geolocation` module is not active
- Switched to [PNPM](http://pnpm.js.org/) for package management for better cross-monorepo support
- **Reflect correct Apache-2.0 licensing** in NPM metadata (was: MIT)
- Updated GraphQL toolchain in dependencies: `@graphql-tools` v6-v8; `@graphql-codegen` v1-v2; `@apollo.client` v2-v3.

## 0.8.4

- Fixed casing of `AgreementResponse.agreement` to remove uppercase `A`
- GraphQL peer dependency minimum compatible version downgraded to `14.5.8`. (Incompatibilities were between GraphQL & GraphiQL, not this lib.)

## 0.8.3

- Added `classifiedAs` to `Organization`
- Added `onHandEffect` to `Action`
- Added `defaultUnitOfResource` to `ResourceSpecification`
- Added `RecipeExchange` to the *recipe* module
- Further modularised schemas to allow economic modules to be used without `Agent` functionality
- Updated GraphQL modules to most recent version (`15.x` series) and configured `graphql` as a peerDependency to allow broader compatibility

## 0.8.2

- Allow overriding options for both `buildASTSchema` and `mergeTypeDefs` 

## 0.8.1

- Allow overriding options to `mergeTypeDefs` in order to deal with looser validation in extension schemas

## 0.8.0

- Added an additional argument to `buildSchema` to allow passing extension schemas as SDL strings in order to extend core VF with custom domain-specific additions easily
- **Breaking:** removed loose `AnyType` custom scalar and restricted `inScopeOf` fields to only allow `Person | Organization` as valid values. Note that implementations may extend the `AccountingScope` union type if they wish to allow other types of record scoping (eg. groups without collective agency, geographical locations).
- **Breaking:** removed `all` prefixes from toplevel record listing endpoints for sensible autocomplete, and made search endpoint query prefixes into suffixes
- **Breaking:** fixed deletion methods taking `String` when they should receive `ID`

## 0.7.1

- Fix generated TypeScript / Flow types missing "bridging" fields due to misconfiguration of `graphql-codegen`
- Fix `EconomicEvent` appreciation edges linking directly to other events instead of via `Appreciation`

## 0.7.0

- Added descriptions to all `input` fields, to make interacting with the API more self-documenting
- Added pagination parameters to all list queries
- Removed many accounting fields from `EconomicEventUpdateParams` that should not have been present
- Added various fields missed in the original conversion:
	- `Agent.primaryLocation`
	- `Scenario.definedAs`
- Fixed missing input fields:
	- `basedOn` & `classifiedAs` in `ProcessUpdateParams`
	- `refinementOf` in `Plan` create / update
	- `resourceConformsTo` in `RecipeResource` create / update
	- `processClassifiedAs` in `RecipeProcess` create / update
	- `refinementOf` in `Scenario` create / update
	- `ScenarioDefinitionUpdateParams.name`
- Add missing mutations & queries for `Claim`, `Scenario`, `ScenarioDefinition` & `SpatialThing`
- Removed `pass` & `fail` actions from the set of core verbs (see [ValueFlows/#610](https://github.com/valueflows/valueflows/issues/610))

## 0.6.1

- Finished some rough edges on modularisation such that you no longer need to explicitly include `query` and `mutation` in the list of schemas to `buildSchema()`.

## 0.6.0

- **Breaking:** significant changes to the internal structure of the module to facilitate modular composition of schemas. Now exports a `buildSchema` function rather than pre-initialised `schema` object. Use `printSchema` on the output of `buildSchema()` for tools which require the input as an SDL schema string, rather than a GraphQLSchema object.

## 0.5.0

- **Breaking:** renames the `transfer-complete` action to `transfer`, as the former was confusing for some users
- Adds missed mutations for `Proposal` and related records

## 0.4.3

- Adds `defaultUnitOfEffort` to `ResourceSpecification` as a stop-gap for unit inference in VF0.4 release (see [#64](https://github.com/valueflows/vf-graphql/issues/64))

## 0.4.2

- Finalise fields for `EconmicResource` & `EconomicEvent` creation & update logic

## 0.4.1

- Adds missed mutations for `Unit` & `ProcessSpecification`

## 0.4.0

**Updated for ValueFlows 0.4 release.**

- Changed from [QUDT](http://www.qudt.org/pages/QUDToverviewPage.html) to [OM](https://github.com/HajoRijgersberg/OM) ontology for measurements
- New action metadata
- Add `EconomicResource` stage & state attributes
- Remove `before` & `after` time fields on `EconomicEvent` & `Process`

## 0.3.0

Initial release. Rough around the edges, missing many mutations & queries, but the core schema is stable.
