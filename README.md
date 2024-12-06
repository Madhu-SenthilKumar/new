
You said:
in the above code the text field for username is rounded shape i want it to be rectangle 

package com.example.expensestracker

import android.content.Context
import android.content.Intent
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.text.font.FontFamily
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import androidx.core.content.ContextCompat
import com.example.expensestracker.ui.theme.ExpensesTrackerTheme

class LoginActivity : ComponentActivity() {
    private lateinit var databaseHelper: UserDatabaseHelper
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        databaseHelper = UserDatabaseHelper(this)
        setContent {
            ExpensesTrackerTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colors.background
                ) {
                    LoginScreen(this, databaseHelper)
                }
            }
        }
    }
}

@Composable
fun LoginScreen(context: Context, databaseHelper: UserDatabaseHelper) {
    Image(
        painterResource(id = R.drawable.download),
        contentDescription = "",
        alpha = 0.2F,
        contentScale = ContentScale.FillHeight
    )

    var username by remember { mutableStateOf("") }
    var password by remember { mutableStateOf("") }
    var error by remember { mutableStateOf("") }

    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {

        Text(
            text = "USER LOGIN",
            fontSize = 36.sp,
            fontWeight = FontWeight.Bold,
            fontFamily = FontFamily.Cursive,
            color = Color(0xFF00796B),
        )

        Spacer(modifier = Modifier.height(20.dp))


        OutlinedTextField(
            value = username,
            onValueChange = { username = it },
            label = {
                Text(
                    text = "Username",
                    color = Color(0xFF333333)
                )
            },
            modifier = Modifier
                .padding(10.dp)
                .width(280.dp)
                .background(Color(0xFFF0F0F0), shape = RoundedCornerShape(16.dp)),
            shape = RoundedCornerShape(16.dp),
            colors = TextFieldDefaults.outlinedTextFieldColors(
                backgroundColor = Color(0xFFF0F0F0),
                focusedBorderColor = Color(0xFF00796B),
                unfocusedBorderColor = Color(0xFFCCCCCC),
                textColor = Color(0xFF333333)
            )
        )

        Spacer(modifier = Modifier.height(10.dp))


        OutlinedTextField(
            value = password,
            onValueChange = { password = it },
            label = {
                Text(
                    text = "Password",
                    color = Color(0xFF333333)
                )
            },
            modifier = Modifier
                .padding(10.dp)
                .width(280.dp)
                .background(Color(0xFFF0F0F0), shape = RoundedCornerShape(32.dp)),
            shape = RoundedCornerShape(32.dp),
            visualTransformation = PasswordVisualTransformation(),
            colors = TextFieldDefaults.outlinedTextFieldColors(
                backgroundColor = Color(0xFFF0F0F0),
                focusedBorderColor = Color(0xFF00796B),
                unfocusedBorderColor = Color(0xFFCCCCCC),
                textColor = Color(0xFF333333)
            )
        )

        Spacer(modifier = Modifier.height(10.dp))

        if (error.isNotEmpty()) {
            Text(
                text = error,
                color = Color(0xFFD32F2F),
                fontWeight = FontWeight.SemiBold,
                fontSize = 14.sp,
                modifier = Modifier.padding(vertical = 8.dp)
            )
        }


        Button(
            onClick = {
                if (username.isNotEmpty() && password.isNotEmpty()) {
                    val user = databaseHelper.getUserByUsername(username)
                    if (user != null && user.password == password) {
                        error = "Logged in successfully"
                        context.startActivity(Intent(context, MainActivity::class.java))
                    } else {
                        error = "Invalid username or password"
                    }
                } else {
                    error = "Please fill all fields"
                }
            },
            modifier = Modifier.padding(top = 16.dp),
            colors = ButtonDefaults.buttonColors(backgroundColor = Color(0xFF00796B))
        ) {
            Text(
                text = "Login",
                color = Color.White,
                fontWeight = FontWeight.Bold,
                fontSize = 18.sp
            )
        }

        Spacer(modifier = Modifier.height(20.dp))

       
        Row {
            TextButton(onClick = {
                context.startActivity(Intent(context, RegisterActivity::class.java))
            }) {
                Text(
                    text = "Sign up",
                    color = Color(0xFF00796B),
                    fontWeight = FontWeight.Medium,
                    fontSize = 16.sp
                )
            }

            Spacer(modifier = Modifier.width(60.dp))

            TextButton(onClick = {}) {
                Text(
                    text = "Forgot password?",
                    color = Color(0xFF00796B),
                    fontWeight = FontWeight.Medium,
                    fontSize = 16.sp
                )
            }
        }
    }
}

private fun startMainPage(context: Context) {
    val intent = Intent(context, MainActivity::class.java)
    ContextCompat.startActivity(context, intent, null)
