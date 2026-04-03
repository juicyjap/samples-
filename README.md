import 'package:flutter/material.dart';

void main() {
runApp(ProInvoiceApp());
}

class ProInvoiceApp extends StatelessWidget {
@override
Widget build(BuildContext context) {
return MaterialApp(
title: 'ProInvoice',
theme: ThemeData(
primarySwatch: Colors.blue,
),
home: DashboardScreen(),
);
}
}

class DashboardScreen extends StatelessWidget {
@override
Widget build(BuildContext context) {
return Scaffold(
appBar: AppBar(
title: Text('ProInvoice'),
),
body: Padding(
padding: EdgeInsets.all(16),
child: Column(
children: [
Card(
child: ListTile(
title: Text('Monthly Earnings'),
subtitle: Text('£0.00'),
),
),
SizedBox(height: 20),
Text('Recent Invoices'),
],
),
),
floatingActionButton: FloatingActionButton(
onPressed: () {
Navigator.push(
context,
MaterialPageRoute(builder: (context) => CreateInvoiceScreen()),
);
},
child: Icon(Icons.add),
),
);
}
}

class CreateInvoiceScreen extends StatefulWidget {
@override
_CreateInvoiceScreenState createState() => _CreateInvoiceScreenState();
}

class _CreateInvoiceScreenState extends State<CreateInvoiceScreen> {
final TextEditingController companyController = TextEditingController();
final TextEditingController clientController = TextEditingController();
final TextEditingController jobController = TextEditingController();

List<Map<String, dynamic>> items = [];

void addItem() {
setState(() {
items.add({"name": "", "qty": 1, "price": 0.0});
});
}

double get total {
double sum = 0;
for (var item in items) {
sum += item["qty"] * item["price"];
}
return sum;
}

@override
Widget build(BuildContext context) {
return Scaffold(
appBar: AppBar(
title: Text('Create Invoice'),
),
body: Padding(
padding: EdgeInsets.all(16),
child: SingleChildScrollView(
child: Column(
children: [
TextField(
controller: companyController,
decoration: InputDecoration(labelText: 'Company Name'),
),
TextField(
controller: clientController,
decoration: InputDecoration(labelText: 'Client Name'),
),
TextField(
controller: jobController,
decoration: InputDecoration(labelText: 'Job / Order Number'),
),
SizedBox(height: 20),
ElevatedButton(
onPressed: addItem,
child: Text('Add Item'),
),
Column(
children: items.map((item) {
return Card(
child: Column(
children: [
TextField(
decoration: InputDecoration(labelText: 'Item Name'),
onChanged: (val) => item["name"] = val,
),
TextField(
decoration: InputDecoration(labelText: 'Qty'),
keyboardType: TextInputType.number,
onChanged: (val) =>
item["qty"] = int.tryParse(val) ?? 1,
),
TextField(
decoration: InputDecoration(labelText: 'Price'),
keyboardType: TextInputType.number,
onChanged: (val) =>
item["price"] = double.tryParse(val) ?? 0.0,
),
],
),
);
}).toList(),
),
SizedBox(height: 20),
Text(
'Total: £${total.toStringAsFixed(2)}',
style: TextStyle(fontSize: 20),
),
ElevatedButton(
onPressed: () {
ScaffoldMessenger.of(context).showSnackBar(
SnackBar(content: Text('Invoice Created')),
);
},
child: Text('Generate Invoice'),
)
],
),
),
),
);
}
}