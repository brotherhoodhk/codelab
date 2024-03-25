# CodeLab
codelab utilites

## Codemaker Usage

### Configure file example
```yaml
Person:
  name: string
  age: int
  isad: bool
Student:
  Person: Person
  Class: string
```
### Generate struct

**smake flags**
- src *source file path*
- language *generate language,(c/go)* 

`codemaker smake --src my.yml --language c`

this command will generate like this.
```c
typedef struct{
int32_t age;
bool isad;
string name;
}Person;
typedef struct{
Person Person;
string Class;
}Student;
```
### Generate jsoncpp/yamlcpp code
`codemaker cpp-gen -i my.yml --mode jsoncpp`
```cpp
template <class T> Json::Value toValue(T* t);
template <> inline Person Json::Value::as<Person>() const{
Person root;
root.name=(*this)["name"].as<string>();
root.age=(*this)["age"].as<int>();
root.isad=(*this)["isad"].as<bool>();
return root;}
template <> Json::Value toValue(Person* object){
Json::Value root;
root["name"]=object->name;
root["age"]=object->age;
root["isad"]=object->isad;
return root;
}
template <> inline Student Json::Value::as<Student>() const{
Student root;
root.Person=(*this)["Person"].as<Person>();
root.Class=(*this)["Class"].as<string>();
return root;}
template <> Json::Value toValue(Student* object){
Json::Value root;
root["Person"]=toValue(&object->Person);
root["Class"]=object->Class;
return root;
}
```
### Notice
The codemaker will not format code and add header file that be generate. You need format it by yourself(you can use clang-format do it) and add the correct header.
