
# Test Rail Flutter

This package is a thin wrapper around the Test Rail API that will allow for automated test reporting in Flutter.

## Getting Started

Initialize the TestRail instance using the config method:

```dart
TestRail.config(
  username: 'USERNAME',
  password: 'PASSWORD',
  /// The url that points to the test rail server => https://example.testrail.com
  serverDomain: 'https://YOUR_SERVER.testrail.com'
)
```

## Usage

### Create or Update Runs

```dart
/// Start by creating a new run
final newRun = await TestRun.create(
  name: 'Test execution',
  projectId: 1
);

/// Add cases to the run
await newRun.updateRun(
  caseIds: [1, 2, 3, 5],
);
```

Once the run is created, results can be reported by case:

```dart
final result = await newRun.addResultForCase(
  caseId: 1,
  statusId: 1,
);

// Optionally add a screenshot or other image to the result
await result.addAttachmentToResult(
  '/workspace/attachments/failure.png',
);
```

### Get

Historical runs, cases, and sections can be retrieved:

```dart
final testCase = await TestCase.get(1);

final testRun = await TestRun.get(1);

final testSection = await TestSection.get(1);
```

## Contributing

# Road Map

There are currently 20 categories of different endpoints that need to be implemented for us
to be at 100% partity with the Test Rail API. With your help as a contributor we can get to 100% in no time.

[Categories](https://gurock.com/testrail/docs/api/reference/)
- Attachments
- Cases
- Case Fields
- Case Types
- Configurations
- Milestones
- Plans
- Priorities
- Projects
- Reports
- Results
- Result Fields
- Runs
- Sections
- Shared Steps
- Statuses
- Suites
- Template
- Tests
- Users

## Contributing

This repo is perfect for people looking to make their first contribution to open source.

You can checkout this guide in order to learn how to make [contributions](https://github.com/firstcontributions/first-contributions)

You can use the app [Quick Type](https://app.quicktype.io/) to autogenerate the expected responses for the models from the above endpoints then you can clean it up to follow our style guide below.

# Dart Style Guide

The purpose of this styling guide is to provide a consistent format to make contributions to our Dart app. We align as much as possible with the best practices set forth by the [Flutter team](https://flutter.dev/docs).
## Writing Unit Tests

Generally speaking, keeping `expect()` as close to one as possible per test helps them stay resilient to refactors. (assert one behavior as it won't break unrelated tests when that functionality changes).

We created a helper *stubTestRailConfig* which you can use in order to pass in sample data for the testcase.

```dart
void main() {
  group('TestSection', () {
    test('#get', () async {
      stubTestRailConfig(sampleTestCase);
      final result = await TestCase.get(1);
      expect(result.asJson, sampleTestCase);
    });
  });
}

## Styling

### Declaration

**_Do_** create declarations that are immutable as much as possible

```dart
/// bad
var response = 'www.dutchie.com';

/// good
final response = 'www.dutchie.com';
const response = 'www.dutchie.com';
```

### Trailing Commas

**_Do_** Add a trailing comma as it makes code more readable when its closing brackets are on a new line

```dart
// bad
TestCase({
  this.createdBy,
  this.createdOn,
  this.suiteId});

/// good
TestCase({
  this.createdBy,
  this.createdOn,
  this.suiteId,
});
```

### Ordering

- instance fields
- static fields
- constructor
- instance methods
- static methods

```dart
class Example {
  final int age;
  final int get dogName {
    return age * 7;
  }
  final String name;
  static const List<String> names = <String>[];

  Example(
    this.age,
    this.name,
  );

  String sayName(String name) {
    print("My name is $name")
  }

  static void saveName(String name) {
    names.add(name);
  }
}
```