# Streams

Q1 -
Given two entity
Customer:
String name;
String email;
List allAddress;

Address:
String city;
String pincode;
Use stream api to fetch all cities from all users(not necessarily distinct)

List<String> sortedCities = customers.stream()
.flatMap(customer -> customer.getAllAddress().stream())
.map(Address::getCity)
.sorted()
.distinct()
.collect(Collectors.toList());
System.out.println(sortedCities); 

Q2 - 


collect duplicates element present in the list using streams

List<Integer> duplicates = numbers.stream()
.collect(Collectors.groupingBy(n -> n, Collectors.counting())) // Count occurrences
.entrySet().stream()
.filter(entry -> entry.getValue() > 1) // Keep elements with count > 1
.map(entry -> entry.getKey()) // Extract keys (duplicate elements)
.collect(Collectors.toList());

Q3

find highest odd element using streams:
Integer odd_highest=
numbers.stream().filter(n->n%2!=0).sorted(Comparator.reverseOrder()).findFirst().orElse(null);

Q4

Find numbers from array which are starting with '1' using stream

List<Integer> result = numbers.stream()
.map(String::valueOf)   // Convert numbers to String
.filter(s -> s.startsWith("1"))  // Filter starting with '1'
.map(Integer::parseInt) // Convert back to Integer
.collect(Collectors.toList());

Q5 

Find max from list 

Integer maxNumber = numbers.stream()
.max(Integer::compareTo).orElse(null);
In case of primitive
int[] numbers = {10, 23, 15, 42, 1, 17, 91};
Correct way: Convert int[] to IntStream and find max 
Integer max = Arrays.stream(numbers).max().orElse(-1);


Q6

Using stream, create a map of <String,List> where Employee is having age and name as attribute, where empployee 
is having ("Ankit",32) ("Mayank",40)("Rahul",32)("Niky","24"). Then I want to group the employees by their age 
e.g. 20-30, 30-40, 40-50, 50-60, 
final map should be "10-20",{} "20-30",("Niky","24"), "30-40",("Ankit",32) ("Mayank",40)("Rahul",32)

List<Employee> employees = Arrays.asList(
new Employee("Ankit", 32),
new Employee("Mayank", 40),
new Employee("Rahul", 32),
new Employee("Niky", 24)
);

        
Map<String, Integer> employeeMap = employees.stream()
                .collect(Collectors.toMap(emp -> emp.name, emp -> emp.age));

       
Map<String, List<Employee>> groupedEmployees = employees.stream()
                .collect(Collectors.groupingBy(emp -> {
                    int age = emp.age;
                    if (age >= 10 && age < 20) return "10-20";
                    else if (age >= 20 && age < 30) return "20-30";
                    else if (age >= 30 && age < 40) return "30-40";
                    else if (age >= 40 && age < 50) return "40-50";
                    else if (age >= 50 && age < 60) return "50-60";
                    else return "60+";
                }));

// Step 4: Ensure all age groups exist in the map
List<String> ageGroups = Arrays.asList("10-20", "20-30", "30-40", "40-50", "50-60");
        ageGroups.forEach(group -> groupedEmployees.putIfAbsent(group, new ArrayList<>()));

// Step 5: Print the results
System.out.println("Employee Map: " + employeeMap);
        groupedEmployees.forEach((ageGroup, empList) ->
                System.out.println(ageGroup + " -> " + empList));



Q7-

Group the numbers within the range and double the numbers 

List<Integer> numbers = Arrays.asList(5, 8, 12, 15, 22, 25, 30);
Group numbers and multiply each number by 2
Map<String, List<Integer>> groupedNumbers = numbers.stream()
                .collect(Collectors.groupingBy(
                        number -> { // Classifier function for grouping
                            if (number <= 10) return "1-10";
                            else if (number <= 20) return "11-20";
                            else return "21-30";
                        },
                        Collectors.mapping(number -> number * 2, Collectors.toList()) // Transform values
                ));
Print the result
System.out.println(groupedNumbers);

Q8 - 

Find the longest string in a list of strings using Java streams:

List<String> strings = Arrays
.asList("apple", "banana", "cherry", "date", "grapefruit");
Optional<String> longestString = strings
.stream()
.max(Comparator.comparingInt(String::length));
// (a, b) -> a.length - b.length;

Q9 -
Calculate the average age of a list of Person objects using Java streams:

List<Person> persons = Arrays.asList(
new Person("Alice", 25),
new Person("Bob", 30),
new Person("Charlie", 35)
);
double averageAge = persons.stream()
.mapToInt(Person::getAge)
.average()
.orElse(0);

Q10 - 

Check if a list of integers contains a prime number using Java streams:

public boolean isPrime(int number) {
if (number <= 1) {
return false;
}
for (int i = 2; i <= Math.sqrt(number); i++) {
if (number % i == 0) {
return false;
}
}
return true;
}

private void printPrime() {
List<Integer> numbers = Arrays.asList(2, 4, 6, 8, 10, 11, 12, 13, 14, 15);
boolean containsPrime = numbers.stream().anyMatch(this::isPrime);
System.out.println("List contains a prime number: " + containsPrime);
}

anyMatch takes a function predicate input 

Q11 - 

Merge two sorted lists into a single sorted list using Java streams:

List<Integer> list1 = Arrays.asList(1, 3, 5, 7, 9);
List<Integer> list2 = Arrays.asList(2, 4, 6, 8, 10);
List<Integer> mergedList = Stream.concat(list1.stream(), list2.stream())
.sorted()
.collect(Collectors.toList());

Q12 - 

Find the intersection of two lists using Java streams:

List<Integer> list1 = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> list2 = Arrays.asList(3, 4, 5, 6, 7);
List<Integer> intersection = list1.stream()
.filter(list2::contains)
.collect(Collectors.toList());

Q13 - 

Remove duplicates from a list while preserving the order using Java streams:

List<Integer> numbersWithDuplicates = Arrays.asList(1, 2, 3, 2, 4, 1, 5, 6, 5);
List<Integer> uniqueNumbers = numbersWithDuplicates
.stream()
.distinct()
.collect(Collectors.toList());

Q14 - 

Given a list of transactions, find the sum of transaction amounts for each day using Java streams:

List<Transaction> transactions = Arrays.asList(
new Transaction("2022-01-01", 100),
new Transaction("2022-01-01", 200),
new Transaction("2022-01-02", 300),
new Transaction("2022-01-02", 400),
new Transaction("2022-01-03", 500)
);

Map<String, Integer> sumByDay = transactions
.stream()
.collect(Collectors.groupingBy(Transaction::getDate,
Collectors.summingInt(Transaction::getAmount)));

Also -  collect takes the collector function
List<Integer> numbers = Arrays.asList(10, 20, 30, 40);
        int sum = numbers.stream()
                .collect(Collectors.summingInt(num -> num)); // Sum of elements

List<String> words = Arrays.asList("Java", "is", "awesome");
        String sentence = words.stream()
                .collect(Collectors.joining(" "));

Q15 - 

Find the kth smallest element in an array using Java streams:

int[] array = {4, 2, 7, 1, 5, 3, 6};
int k = 3; // Find the 3rd smallest element
int kthSmallest = Arrays.stream(array)
.sorted()
.skip(k - 1)
.findFirst()
.orElse(-1);

Q16 - 

Implement a method to partition a list into two groups based on a predicate using Java streams:

List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
Map<Boolean, List<Integer>> partitioned = numbers
.stream()
.collect(Collectors.partitioningBy(n -> n % 2 == 0));
partioningBy always create map with true and false

Q17 

Sum of All Elements in a List

int sum = numbers.stream()
.mapToInt(num -> num)
.sum();

Q18

Count Elements Greater Than a Given Value

long count = numbers.stream()
.filter(n -> n > 3)
.count();

Q19 

Find Second Highest Number in a List

Optional<Integer> secondHighest = numbers.stream()
.distinct()
.sorted(Comparator.reverseOrder())
.skip(1)
.findFirst();

Q20
Flatten a List of Lists into a Single List

List<Integer> flattenedList = listOfLists.stream()
.flatMap(list -> list.stream())
.collect(Collectors.toList());

Q21
Find the Most Frequent Word in a List

words.stream().collect(Collectors.groupingBy(w->w, Collectors.counting()))
.entrySet().stream().sorted((a,b) -> (int)(b.getValue() -
a.getValue())).findFirst().get().getKey();

Q22

Check If All Elements in a List Are Even

boolean allEven = numbers.stream()
.allMatch(n -> n % 2 == 0);

Q23

Find the Maximum and Minimum Values in a List

Optional<Integer> max = numbers.stream().max(Integer::compareTo);
Optional<Integer> min = numbers.stream().min(Integer::compareTo);

Optional<Integer> max = numbers.stream().max(Comparator.comparingInt(Integer::valueOf));

Q24

Sort a List of Strings by Length

List<String> sortedByLength = names.stream()
.sorted(Comparator.comparingInt(String::length))
.collect(Collectors.toList());

Q25 

Find Department with Highest Average Salary

Map.Entry<String, Double> highestPaidDept = employees.stream()
.collect(Collectors.groupingBy(emp -> emp.department, Collectors.averagingDouble(emp -> emp.salary)))
.entrySet()
.stream()
.max(Map.Entry.comparingByValue())
.get();

Q26 

First Unique character in string 

Optional<Character> firstUniqueChar = input.chars()
.mapToObj(c -> (char) c)
.collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new, Collectors.counting()))
.entrySet()
.stream()
.filter(entry -> entry.getValue() == 1)
.map(Map.Entry::getKey)
.findFirst();

Q27 

Parallel Streams improve performance using multiple threads.
Uses ForkJoinPool for splitting & processing data chunks.
Works well for large data sets & computationally expensive tasks.
Can lead to unexpected ordering issues or race conditions.

List<String> sortedNames = names.parallelStream()
.filter(name -> name.length() > 3)
.sorted()
.collect(Collectors.toList());


Grouping employees by department using parallel stream
        Map<String, List<Employee>> departmentGroups = employees.parallelStream()
                                                                .collect(Collectors.groupingBy(emp -> emp.department));













