# Taplytics Java SDK

Taplytics Java SDK enables you to use deliver server-side experiments, feature flags and functionality at edge.

**[Get started with Taplytics](https://docs.taplytics.com/docs/java-sdk)** | **[View the Javadoc](https://s3.amazonaws.com/cdn.taplytics.com/javasdk/javadoc/com/taplytics/sdk/package-summary.html)** |  **[Commercial License / Terms](http://taplytics.com/terms)**

## Initialization

First, import the taplytics-java-sdk JAR into your project.

Then, import the API client into the desired class:
```java
import com.taplytics.sdk.APIController;
```

API client can be then initialized as following:

```java
APIController taplytics = new APIController('API_KEY');
```

## Identification

All method calls to Taplytics require a user id. This is to ensure that the user will always receive the correct experiments and variations across all platforms.

## Methods

All methods can be handled with a callback, or used as a CompletableFuture, i.e. 

```java
try {
  JSONObject config = taplytics.getConfig(...).get();
  // use config
} catch (ExecutionException e) {
  // error e
}
```
If a callback is provided, the function will not return a promise.

### getBucketing

> Returns a key/value pairing of all experiments that the user has been segmented into, as well as the variation the users are assigned.

#### Parameters

| Parameter | Tags | Description |
|-----------|------|-------------|
| userId |  ``` Required ```  | ID for current user |
| attributes |  ``` Optional ```  | Provide all relevant attributes associated with the user |
| callback |  ``` Optional ```  | Provide a callback if you don't want to use a promise|


#### Example Usage

```java
String userId = "user_id";
JSONObject attributes = new JSONObject();
attributes.put("key", "value");

taplytics.getBucketing(userId, attributes)
	.handle((bucketing, err) -> {
		// your code here
}).join();
```


### getVariables

> All variables and their values for the given user

#### Parameters

| Parameter | Tags | Description |
|-----------|------|-------------|
| userId |  ``` Required ```  | ID for given user |
| attributes |  ``` Optional ```  | All relevant attributes associated with the user |
| callback |  ``` Optional ```  | Provide a callback if you don't want to use a promise|


#### Example Usage

```java
String userId = "user_id";
JSONObject attributes = new JSONObject();
attributes.put("key", "value");

taplytics.getVariables(userId, attributes)
	.handle((variables, err) -> {
		// your code here
}).join();
```

### getVariationForExperiment

> For a given experiment, determine whether or not a user is in the experiment, and in which variation


#### Parameters

| Parameter | Tags | Description |
|-----------|------|-------------|
| userId |  ``` Required ```  | ID for given user |
| experimentName |  ``` Required ```  | Name of an Experiment |
| attributes |  ``` Optional ```  | All relevant attributes associated with the user |
| callback |  ``` Optional ```  | Provide a callback if you don't want to use a promise|


#### Example Usage

```java
String userId = "user_id";
String experimentName = "experimentName";
JSONObject attributes = new JSONObject();
attributes.put("key", "value");

taplytics.getVariationForExperiment(userId, experimentName, attributes)
	.handle((variation, err) -> {
		// your code here
}).join();
```


### getVariableValue


> Value for given Taplytics Dynamic Variable. If a user is not in an experiment containing the variable, the response be the provided default value in the query params.


#### Parameters

| Parameter | Tags | Description |
|-----------|------|-------------|
| userId |  ``` Required ```  | ID for given user |
| varName |  ``` Required ```  | name of variable |
| defaultValue |  ``` Required ```  | default value to be used if user does not have variable available. |
| attributes |  ``` Optional ```  | All relevant attributes associated with the user |
| callback |  ``` Optional ```  | Provide a callback if you don't want to use a promise|

#### Example Usage

```java
String userId = "user_id";
String varName = "varName";
String defaultValue = "defaultValue";

JSONObject attributes = new JSONObject();
attributes.put("key", "value");

taplytics.getVariableValue(userId, varName, defaultValue, attributes)
	.handle((value, err) -> {
		// your code here
}).join();
```

### sendEvent

> Send an event to Taplytics. These events are then used to compare against an experiment's goals to determine the success of an A/B test.

#### Parameters

| Parameter | Tags | Description |
|-----------|------|-------------|
| userId |  ``` Required ```  | ID for given user |
| body |  ``` Optional ```  | Provide an array of events, as well as all additional relevant user attributes. |
| callback |  ``` Optional ```  | Provide a callback if you don't want to use a promise|

#### Example Usage

```java
String userId = "user_id";
JSONObject body = new JSONObject();

JSONObject event = new JSONObject();
event.put("eventName", "event1");
event.put("eventValue", 3);

JSONArray events = new JSONArray();
events.put(event);
body.put("events", events);

taplytics.sendEvent(userId, body)
	.handle((response, err) -> {
		// your code here
}).join();
```

### getConfig

> Returns the entire configuration for the project. This is the document that informs the experiment information such as segmentation. Extremely verbose and should be used for debugging.

#### Parameters

| Parameter | Tags | Description |
|-----------|------|-------------|
| userId |  ``` Required ```  | ID for given user |
| attributes |  ``` Optional ```  | All relevant attributes associated with the user |
| callback |  ``` Optional ```  | Provide a callback if you don't want to use a promise|

#### Example Usage

```java
String userId = "user_id";
JSONObject attributes = new JSONObject();
attributes.put("key", "value");

taplytics.getConfig(userId, attributes)
	.handle((config, err) -> {
		// your code here
}).join();
```

### getFeatureFlags

> Returns array of all the enabled feature flags for this project. Each object in the array has `name` for the name of the feature flag and `keyName` for the feature flag's key to be used in your code

#### Parameters

| Parameter | Tags | Description |
|-----------|------|-------------|
| userId |  ``` Required ```  | ID for given user |
| attributes |  ``` Optional ```  | All relevant attributes associated with the user |
| callback |  ``` Optional ```  | Provide a callback if you don't want to use a promise|

#### Example Usage

```java
String userId = "user_id";
JSONObject attributes = new JSONObject();
attributes.put("key", "value");

taplytics.getFeatureFlags(userId, attributes
	.handle((featureFlags, err) -> {
		// your code here
}).join();
```

### isFeatureFlagEnabled

> Returns true or false depending if the `keyName` provided corresponds to an enabled featureFlag in your Taplytics project

#### Parameters

| Parameter | Tags | Description |
|-----------|------|-------------|
| userId |  ``` Required ```  | ID for given user |
| keyName |  ``` Required ```  | Key name for the feature flag |
| attributes |  ``` Optional ```  | All relevant attributes associated with the user |
| callback |  ``` Optional ```  | Provide a callback if you don't want to use a promise|

#### Example Usage

```java
String userId = "user_id";
JSONObject attributes = new JSONObject();
attributes.put("key", "value");

taplytics.isFeatureFlagEnabled(userId, attributes)
	.handle((enabled, err) -> {
		if ((Boolean) enabled) {
			enableFeature();
		}
}).join();
```
