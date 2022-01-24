**main.dart file**

import 'package:flutter/material.dart';

import 'Screens/AllCountries.dart';

void main(){
  runApp(new MaterialApp(
    home: new AllCountries(),
  ));
}

**AllCountries.dart file**

import 'Country.dart';
import 'package:flutter/material.dart';
import 'package:dio/dio.dart';     
        
class AllCountries extends StatefulWidget {

  @override
  State<AllCountries> createState() => _AllCountriesState();
}

class _AllCountriesState extends State<AllCountries> {
 late Future<List> countries;
  Future<List> getCountries() async{
    var response = await Dio().get("https://restcountries.com/v3.1/all");     
         return response.data;   
    
  }

  @override
  void initState() {
    countries = getCountries();
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
   print(countries);                                              
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.blue,
        title: const Text('All Countries'),
      ),
      body: Container(
        padding: const EdgeInsets.all(10),
        child: FutureBuilder<List>(
        future: countries,
        builder: (BuildContext context, AsyncSnapshot<List> snapshot) {
          if (snapshot.hasData) {
            return ListView.builder(itemBuilder: (BuildContext context, int index){
                return GestureDetector(
          onTap: (){
            Navigator.of(context).push(
              MaterialPageRoute(
                builder: (context) => Country("India"),                   
              ),
            );
          },
          child: const Card(
            elevation: 10,
            child: Padding(
              padding: EdgeInsets.symmetric(
                vertical: 10, horizontal: 8),
              child: Text(
                //'India',
                snapshot.data[index]["name"],                       //error 
                style: TextStyle(fontSize: 20),
                ),
            ),
          ),
        );
            });
          }
           return Country("India");
          },
            )

        /* ListView(children: [
           GestureDetector(
          onTap: (){
            Navigator.of(context).push(
              MaterialPageRoute(
                builder: (context){   
                  return Country("India");
                },                         
              ),
            );
          },
          child: const Card(
            elevation: 10,
            child: Padding(
              padding: EdgeInsets.symmetric(vertical: 10, horizontal: 8),
              child: Text(
                'India',
                style: TextStyle(fontSize: 20),
                ),
            ),
          ),
        ),
        GestureDetector(
          onTap: (){
            Navigator.of(context).push(
              MaterialPageRoute(
                builder: (context){   // builder: (context) => Country(),
                  return Country("Canada");
                },                         //
              ),
            );
          },
          child: const Card(
            elevation: 10, 
            child: Padding(
              padding: EdgeInsets.symmetric(vertical: 10, horizontal: 8),
              child: Text(
                'Canada',
                style: TextStyle(fontSize: 20),
                ),
            ),
          ),
        ),
        ],) */ 
      ),
    );
  }
}
  
 **Country.dart file **
  
  import 'package:flutter/material.dart';

class Country extends StatelessWidget {
  final String name;
  Country(this.name);

  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Colors.blue,
        title: Text(name),
      ),
      
    );
  }
}
