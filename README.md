# Universal Presentation: A Header-only C++ Library to Print STL containers and more

This library, UV (Universal Presentation), helps you to `std::cout` most C++ STL containers in addition to the primitive types, as long as the underlying object `T` is *cout-able*.

## Usage

### Primitive Types

UV can prints primitive types with customized control.

``` c++
std::vector<int> vec{1, 2, 3, 4, 5};
std::cout << util::pre(vec) << std::endl;
// [1, 2, 3, 4, 5]

std::cout << util::pre(vec, true /* verbose */, 2 /* limit */) << std::endl;
// [1, ..., 4] (sz: 5, ommitted 3)

std::cout << util::pre(vec, false, 0) << std::endl;
// [...]
```

### Custom Types

UV can also handle any custom types as long as it is *cout-able*.

For example, `Person` is a cout-able object that overwrites `operator<<`.

``` c++
struct Person
{
    std::string name;
    int age;
};

std::ostream &operator<<(std::ostream &os, const Person &p)
{
    os << "{Person " << p.name << " age: " << p.age << "}";
    return os;
};
```

In this case, UV handles the output of most STL containers well.

For vector

``` c++
std::vector<Person> persons{
    Person{.name = "A", .age = 25},
    Person{.name = "B", .age = 30},
    Person{.name = "C", .age = 45},
};
std::cout << util::pre(persons) << std::endl;
// [{Person A age: 25}, {Person B age: 30}, {Person C age: 45}]

```

For map

``` c++
std::map<std::string, Person> m;
m["boss"] = Person{.name = "A", .age = 70};
m["employee"] = Person{.name = "B", .age = 30};
m["employee2"] = Person{.name = "C", .age = 30};
m["employee4"] = Person{.name = "D", .age = 30};
std::cout << "map: " << util::pre(m) << std::endl;
// map: {("boss", {Person A age: 70}), ("employee", {Person B age: 30}), ("employee2", {Person C age: 30}), ("employee4", {Person D age: 30})}
```

For two-dimention vector

``` c++
std::vector<std::vector<Person>> matrix;
matrix.push_back(
    {Person{.name = "A", .age = 10}, Person{.name = "B", .age = 20}});
matrix.push_back(
    {Person{.name = "C", .age = 30}, Person{.name = "D", .age = 40}});
std::cout << "matrix: " << util::pre(matrix) << std::endl;
// [[{Person A age: 10}, {Person B age: 20}], [{Person C age: 30}, {Person D age: 40}]]
```

## One More Thing

UV gives more friendly outputs to C-style types and specific STL utilities.


Pair and Tuple: 

``` c++
std::pair<int, int> pair{100, 200};
std::cout << "pair: " << util::pre(pair) << std::endl;
// pair: (100, 200)

std::tuple<Person, int, int, int> t{
    Person{.name = "tuple", .age = 4}, 2, 3, 4};
std::cout << "tuple: " << util::pre(t) << std::endl;
// tuple: <{Person tuple age: 4}, 2, 3, 4>
```


UV recognizes C-style array, but does not mess up with C-style string:

``` c++
int c_array[] = {2, 4, 6, 8, 10};
std::cout << "c style array: " << util::pre(c_array) << std::endl;
// c style array: [2, 4, 6, 8, 10]
std::cout << "c style array: " << util::pre(c_array, false, 1) << std::endl;
// c style array: [2, ...]
std::cout << "c style string: " << util::pre("hello world") << std::endl;
// c style string: "hello world"
// ** NOTE: will not show the ugly ['h', 'e', 'l', ...] version.

```

UV gives more friendly outputs to legacy C types.

``` c++
std::cout << "c style string: " << util::pre("hello world") << std::endl;
// c style string: "hello world"
std::cout << "char: " << util::pre('a') << std::endl;
// char: 'a'
std::cout << "boolean: " << util::pre(true) << std::endl;
// boolean: true
```

## Compile

``` bash
mkdir build; cd build
cmake -DCMAKE_BUILD_TYPE=Debug ..
# or cmake -DCMAKE_BUILD_TYPE=Release ..
# for more options, see CMakeLists.txt
make -j
```

## Run

``` bash
$ ./bin/main
```