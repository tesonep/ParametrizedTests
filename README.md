This is an small implementation of parametrized tests for Pharo.
It allows to generate different test cases for each of the possible configurations in a matrix.

# Installing

To use it, it should be installed in Pharo. To install it with Metacello execute

```Smalltalk

	Metacello new
	  baseline: 'ParametrizedTests';
	  repository: 'github://tesonep/parametrizedTests';
	  load.
	
```

# Using

Once installed, there is a new subclass of *TestCase* called *ParametrizedTestCase*.
This is the class all the test cases using parametrized tests should use.
It extends the default *TestCase* with support for parametrized tests.

A parametrized tests is a test that is run with different configurations.
These configurations are generated from a Matrix.

To Parametrized tests just extend this class as a normal tests, 
add accessors for the properties to configure and implement the class side method #testParameters.

When the test class is executed a TestSuite is generated with an instance for each of the configured cases. This instances are populated using the accessors to the described properties. 

The class side method #testParameters returns a test matrix. 
This test matrix can be generated with cartesian product configurations or a set of well known cases.

An example of #testParameters is: 

```Smalltalk
testParameters

	^ ParametrizedTestMatrix new
		forSelector: #option1 addOptions: #(a b c);
		forSelector: #option2 addOptions: {[1].[2].[3]};
		yourself.
``` 

Each option is constituted from a set of possible values and a selector that is the name of the property to set in the test case instance.

Also each option can be a literal or a block to generate that value. The block has an optional parameter, the parameter is the test case to configure.

In this example the generated cases will be 9. One per each combination of #option1 and #option2. 
The test Case should have a setter for 

Check the Examples to see how to use the different possible configurations.

# Examples

The API is quite simple, but there are two simple examples to show the use of generated matrix and set of cases.
The example class *PaSimpleMatrixExampleTest* shows an example of a matrix. 

```Smalltalk
testParameters

	^ ParametrizedTestMatrix new
		forSelector: #item1 addOptions: { 1. 'a'. $c };
		forSelector: #item2 addOptions: { 2. 'b'. $d };
		forSelector: #collectionClass addOptions: 	{ Set. Bag. OrderedCollection }
```

This example generates 81 different cases, with all the combinations. 

The second example uses a set of given cases. This example is shown in the test class *PaSelectedCasesExampleTest*

```Smalltalk
testParameters

	^ ParametrizedTestMatrix new
		addCase: { #number1 -> 2. #number2 -> 1.0. #result -> 3 };
		addCase: { #number1 -> (2/3). #number2 -> (1/3). #result -> 1 };
		yourself
```

This example generates exactly the two cases listed in the test.