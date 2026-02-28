<img width="180" height=320" alt="Screenshot_20260228_171751" src="https://github.com/user-attachments/assets/873278f5-ec43-41f1-b2fd-22ff6464d21a" />


@Composable
fun MyApp() {
    Surface(
        modifier = Modifier.fillMaxSize(),
        color = MaterialTheme.colorScheme.background
    ) {
        DollarCounter()
    }
}

@Composable
fun DollarCounter() {
//Parent Composable er moddhe state Hold kora ase
    var counter by remember { mutableIntStateOf(1) }

    Column(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(text = "$${counter * 100}", style = MaterialTheme.typography.bodyLarge)
        Spacer(modifier = Modifier.height(180.dp))
        CustomButton(onClick = { counter++ })
    }
}
এখানে DollarCounter হলো parent।

এটি state (counter) ধরে রেখেছে।

Child‑কে শুধু state update করার জন্য callback পাঠাচ্ছে।


//Child Composable
// stateless child //child ke parent class stateless kore ter  mane stateless korle ashole ki hobe
@Composable
fun CustomButton(onClick: () -> Unit) {
    Card(
        modifier = Modifier
            .size(120.dp)
            .clickable { onClick() },
        shape = CircleShape,
       // backgroundColor = Color.Yellow meterial 2 deprecated
        colors = CardDefaults.cardColors(containerColor = Color.Yellow)
    ) {
        Box(
            modifier = Modifier.fillMaxSize(),
            contentAlignment = Alignment.Center
        ) {
            Text(
                text = "Tap",
                style = TextStyle(
                    color = Color.Black,
                    fontSize = 20.sp,
                    fontWeight = FontWeight.Bold
                )
            )
        }
    }
}


এখানে CustomButton হলো child।
Child কোনো state জানে না, শুধু UI দেখায়।
Parent থেকে আসা onClick callback invoke করে।

Flow Diagram (Textual)
Code
DollarCounter (Parent)
   ├── holds state (counter)
   ├── passes onClick callback
   ▼
CustomButton (Child)
   ├── shows UI
   └── invokes onClick → Parent updates counter
👉 তাই তোমার কোডে DollarCounter = Parent আর CustomButton = Child।
Parent state manage করছে, Child stateless থেকে শুধু UI render করছে।
