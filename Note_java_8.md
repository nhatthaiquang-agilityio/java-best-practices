#Java 8 Lambda

1. Event handling using Java 8 Lambda expressions

	```
	// Before Java 8:
	JButton show = new JButton("Show");
	show.addActionListener(new ActionListener() {
		@Override
		public void actionPerformed(ActionEvent e) {
			System.out.println("Event handling without lambda expression is boring");
		}
	});

	// Java 8 way:
	show.addActionListener((e) -> {
		System.out.println("Light, Camera, Action !! Lambda expressions Rocks");
	});
	```

2. Iterating over List

	```
	//Prior Java 8 :
	List features = Arrays.asList("Lambdas", "Default Method", "Stream API", "Date and Time API");
	for (String feature : features) {
		System.out.println(feature);
	}

	//In Java 8:
	List features = Arrays.asList("Lambdas", "Default Method", "Stream API", "Date and Time API");
	features.forEach(n -> System.out.println(n));

	// Even better use Method reference feature of Java 8
	// method reference is denoted by :: (double colon) operator
	// looks similar to score resolution operator of C++
	features.forEach(System.out::println);

	```

3. Functional interface Predicate

	```
	public static void main(args[]){
		List languages = Arrays.asList("Java", "Scala", "C++", "Haskell", "Lisp");
		System.out.println("Languages which starts with J :");

		filter(languages, (str)->str.startsWith("J"));
		System.out.println("Languages which ends with a ");

		filter(languages, (str)->str.endsWith("a"));
		System.out.println("Print all languages :");

		filter(languages, (str)->true);
		System.out.println("Print no language : ");

		filter(languages, (str)->false);
		System.out.println("Print language whose length greater than 4:"); filter(languages, (str)->str.length() > 4);
	}

	public static void filter(List names, Predicate condition) {
		for(String name: names) {
			if(condition.test(name)) {
				System.out.println(name + " ");
			}
		}
	}

	//Even better
	public static void filter(List names, Predicate condition) {
		names.stream().filter((name) -> (condition.test(name))).forEach((name) -> {
			System.out.println(name + " ");
		});
	}

	```

4. Map and Reduce

	```
	// applying 12% VAT on each purchase
	// Without lambda expressions:
	List costBeforeTax = Arrays.asList(100, 200, 300, 400, 500);
	for (Integer cost : costBeforeTax) {
		double price = cost + .12*cost;
		System.out.println(price);
	}

	// With Lambda expression:
	List costBeforeTax = Arrays.asList(100, 200, 300, 400, 500);
	costBeforeTax.stream().map((cost) -> cost + .12*cost).forEach(System.out::println);

	```


	```
	// Applying 12% VAT on each purchase
	// Old way:
	List costBeforeTax = Arrays.asList(100, 200, 300, 400, 500);
	double total = 0;
	for (Integer cost : costBeforeTax) {
		double price = cost + .12*cost; total = total + price;
	}

	System.out.println("Total : " + total);

	// New way:
	List costBeforeTax = Arrays.asList(100, 200, 300, 400, 500);
	double bill = costBeforeTax.stream().map((cost) -> cost + .12*cost).reduce((sum, cost) -> sum + cost).get();
	System.out.println("Total : " + bill);
	```

5. Applying function on Each element of List

	```
	// Convert String to Uppercase and join them using coma
	List<String> G7 = Arrays.asList("USA", "Japan", "France", "Germany", "Italy", "U.K.","Canada");
	String G7Countries = G7.stream().map(x -> x.toUpperCase()).collect(Collectors.joining(", "));
	System.out.println(G7Countries);
	```

6. Creating a Sub List by Copying distinct values

	```
	// Create List of square of all distinct numbers
	List<Integer> numbers = Arrays.asList(9, 10, 3, 4, 7, 3, 4);
	List<Integer> distinct = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
	System.out.printf("Original List : %s, Square Without duplicates : %s %n", numbers, distinct);

	```