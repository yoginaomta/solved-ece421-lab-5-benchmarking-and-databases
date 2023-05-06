Download Link: https://assignmentchef.com/product/solved-ece421-lab-5-benchmarking-and-databases
<br>
<h1>Benchmarking in Rust</h1>

Consider the following  x86 machine code:

Details of x86 machine code can be found at:

<a href="http://flint.cs.yale.edu/cs421/papers/x86-asm/asm.html">http://flint.cs.yale.edu/cs421/papers/x86</a><a href="http://flint.cs.yale.edu/cs421/papers/x86-asm/asm.html">–</a><a href="http://flint.cs.yale.edu/cs421/papers/x86-asm/asm.html">asm/asm.html</a>

and also, at:

<a href="https://www.youtube.com/watch?v=75gBFiFtAb8">https://www.youtube.com/watch?v=75gBFiFtAb8</a>

<table width="628">

 <tbody>

  <tr>

   <td width="628">.LCPI0_0:.long   10         .long   0.long   12         .long   32 example::main:sub     rsp, 16         movaps  xmm0, xmmword ptr [rip + .LCPI0_0]         movups  xmmword ptr [rsp], xmm0         xor     eax, eax         cmp     dword ptr [rsp + 4], 32         setg    al         mov     dword ptr [rsp + 8*rax + 4], 10         add     rsp, 16         ret</td>

  </tr>

 </tbody>

</table>

<strong>Question 1: </strong>Examine the machine code and explain what it does? <em>(Hint: What can you do to a list with two for loops and a single conditional?) </em><strong>Question 2:</strong> Translate the machine code to Rust.

<strong>Question 3:</strong> Use the compiler explorer (<a href="https://godbolt.org/">https://godbolt.org/)</a> to generate the machine code for your answer in Question 2.

<strong>Question 4: </strong>Does the optimization flag -O make a difference?

<ul>

 <li><strong>DEMO this deliverable to the lab instructor. </strong></li>

</ul>

Criterion (https://github.com/bheisler/criterion.rs) is a statistics-driven micro benchmarking crate in Rust. It helps you write fast code by detecting and measuring performance improvements or regressions, even small ones, quickly and accurately. The crate helps us optimize with confidence, knowing how each change affects the performance of our code.

<strong>Question 5:</strong> Follow the instructions in the quickstart section

(https://github.com/bheisler/criterion.rs#quickstart ) to add the Criterion crate to your project (<strong>Question 2</strong>).

<strong>Question 6:</strong> Bench your function from Question 2 using the following code:

<table width="628">

 <tbody>

  <tr>

   <td width="628">let mut rng = rand::thread_rng(); let mut l: Vec&lt;i64&gt; = (0..10000).map(|_| {rng.gen_range(1, 10000)}).collect();c.bench_function(“your function: “, |b| b.iter(|| <strong>your_function</strong>::&lt;i64&gt;(black_box(&amp;mut l))));</td>

  </tr>

 </tbody>

</table>

Feel free to replace “your_code” with the name of the function you wrote in Question 2 and examine the output of the benchmark.

<ul>

 <li><strong>DEMO this deliverable to the lab instructor. </strong></li>

</ul>

<h1>Part 2: Code Optimization</h1>

Based on the results obtained in Question 6, Do you see any room for optimization<em>? (Hint: consider using iterators (</em><a href="https://doc.rust-lang.org/std/iter/trait.Iterator.html"><em>https://doc.rust</em></a><a href="https://doc.rust-lang.org/std/iter/trait.Iterator.html"><em>–</em></a><a href="https://doc.rust-lang.org/std/iter/trait.Iterator.html"><em>lang.org/std/iter/trait.Iterator.html</em></a><a href="https://doc.rust-lang.org/std/iter/trait.Iterator.html"><em>)</em></a><em>  </em><strong>Question 7: </strong>Optimize your code from Question 2.

<strong>Question 8:</strong> Bench your function from Question 7 using the following code:

<table width="628">

 <tbody>

  <tr>

   <td width="628">let mut rng = rand::thread_rng(); let mut l: Vec&lt;i64&gt; = (0..10000).map(|_| {rng.gen_range(1, 10000)}).collect();c.bench_function(“your function: “, |b| b.iter(|| <strong>your_function</strong>::&lt;i64&gt;(black_box(&amp;mut l))));</td>

  </tr>

 </tbody>

</table>

<strong>Question 9: </strong>Compare between the two performance results obtained from Criterion crate from Question 6 and Question 8.

<strong>Question 10:</strong> What are “zero-cost abstractions” in Rust? Explain how this term applies to Question 9.

<ul>

 <li><strong>DEMO (and Explain) this deliverable to the lab instructor. </strong></li>

</ul>

<h1>Part 3: Managing Databases</h1>

Modern applications are rapidly evolving to become data-heavy systems. For example, Training machines with data is starting to become a mainstream way to solve complex problems. As a result, the data should be easy to retrieve and must have guarantees regarding consistency and durability. Databases are the go-to solution for providing a robust foundation for building applications that are data-backed and that support the expectations of their users. A database is a collection of tables. Tables are units of data organization. Data that’s been organized into tables is only applicable to relational databases.  Other databases, such as NoSQL and graphbased databases, use a more flexible document model to store and organize data. Tables are usually representations of entities from the real world. These entities can have various attributes that take the place of columns in these tables.

In this part, we will use SQLite to design a convenient database system to provide data storage for a simple Bank application written in Rust.

<h2>1- SQLite Integration</h2>

<strong>First</strong>, we need need to install SQLite in order to be able to create our database.

<ul>

 <li><u>For windows</u>:

  <ul>

   <li>Install SQLite for their website (<a href="https://www.sqlite.org/download.html">https://www.sqlite.org/download.html</a><a href="https://www.sqlite.org/download.html">)</a>.</li>

   <li>Download the sqlite-shell-win32-*.zip and sqlite-dll-win32-*.zip zipped files.</li>

   <li>Create a folder on your machine (e.g., C:sqlite) and unzip these two zipped files in this folder.</li>

   <li>Add the path for this folder (e.g., C:sqlite) to your PATH envrionment variable.</li>

   <li>To test the installation, go to the terminal and write sqlite3 comand, the following message should be displayed.</li>

  </ul></li>

</ul>




<ul>

 <li><u>For Linux:</u></li>

</ul>

SQLite should be installed by default on almost all the flavours of Linux OS. You can test the installation by issuing the <em>sqlite3</em> command in the terminal.

<ul>

 <li><u>For Mac OS:</u>

  <ul>

   <li>Although the lates version of Mac should have SQLite installted by default, you can insall it from their website (<a href="https://www.sqlite.org/download.html">https://www.sqlite.org/download.html</a><a href="https://www.sqlite.org/download.html">)</a>.</li>

   <li>Download sqlite-autoconf-*.tar.gz from source code section.</li>

   <li>Run the following command −</li>

  </ul></li>

</ul>

$tar xvfz sqlite-autoconf-3071502.tar.gz

$cd sqlite-autoconf-3071502

$./configure –prefix=/usr/local

$make

$make install

<ul>

 <li>To test the intallation, run the following command on your machnie:</li>

</ul>

$sqlite3

<strong> </strong>

<strong> </strong>

<strong> </strong>

<strong> </strong>

<strong>Second</strong>, once you have SQLite installed, you can create a new Rust project, then create a new folder called data. Your project’s folders should have the following folders/files:

<ul>

 <li>Cargo has generated a new directory hello-rust with the following files:</li>

</ul>

DBProject

├── Cargo.toml

└── data

└── src

└── main.rs

1 directory, 2 files

Navigate to the data folder and run the following command to create your database

sqlite3 users.db

Your database will have two tables:

<ul>

 <li>One for username and password</li>

 <li>Another one to store a list of transactions.</li>

 <li>To create the user table that stores usernames and passwords for bank clients, we can run the following command:</li>

</ul>

sqlite&gt; create table users(u_name text PRIMARY KEY, p_word text);

To display a list of tables in your database, you can use:

sqlite&gt; .tables

This should return:

users

<ul>

 <li>the transactions table should contain the following fields:</li>

 <li>u_from: the user from which the transaction is sent.</li>

 <li>u_to: the user to which the transaction is going.</li>

 <li>t_date: the transaction date</li>

 <li>t_amount: the transaction amount</li>

 <li>The primary key is using multiple names combining u_from and t_date.</li>

 <li>To create the table:</li>

</ul>

sqlite&gt; create table transactions(u_from text, u_to text, t_date integer, t_amount text, PRIMARY KEY(u_from,t_date), FOREIGN KEY (u_from) REFERENCES users(u_name), FOREIGN KEY (u_to) REFERENCES users(u_name));

<ul>

 <li>The table has two foreign keys that reference the u_name from the users table.</li>

</ul>




Now running the .tables command:

sqlite&gt; .tables

should return:

transactions  users

<strong>Third</strong>, to insert data into the users database, we can use the insert statemetn as follows:

sqlite&gt; insert into users (u_name, p_word) values (“Matt”, “matt_pw”), (“Dave”,

“dave_pw”);

sqlite&gt; insert into transactions (u_from, u_to, t_date, t_amount) values (“Dave”,”Matt”,datetime(“now”),50);

We can use the phrase datetime(“now”), which is a SQL function to return the current time.

To view the data, we can use the select statement:

<strong>sqlite&gt; select * from users; </strong>

Matt|matt_pw Dave|dave_pw <strong>sqlite&gt; select * from transactions; </strong>

Dave|Matt|2019-10-15 01:54:09|50

To delete from your tables, you can use:

sqlite&gt; delete from transactions; sqlite&gt; delete from users;

<h2>2- Data Management using Rust</h2>

<strong>First</strong>, we should not save passwords as text. To make secure passwords, we will use the bcrypt library:




From that bcrypt, we will need the methods: DEFAULT_COST, hash, verify; and the error type BcryptError.

Also, to manage our SQLite database from our code, we need SQLite crate:




<strong>Second</strong>, we will need to create a struct <em>UserBase</em> to handle access to the database. So, we will add the following to our <em>main.rs</em> file

pub struct UserBase{

fname:String,

}

We are going to combine two different error types: bcrypt errors and the SQLite errors. We can use another OR structure by implementing the erros as enum:

<table width="628">

 <tbody>

  <tr>

   <td width="628">use bcrypt::{DEFAULT_COST, hash, verify, BcryptError}; use sqlite::Error as SqErr;#[derive(Debug )] pub enum UBaseErr{DbErr(SqErr),HashError(BcryptError)}</td>

  </tr>

 </tbody>

</table>

To handle both types of error as UBaseErr, we can use the From trait allows for a type to define how to create itself from another type, hence providing a very simple mechanism for converting between several types.

<table width="628">

 <tbody>

  <tr>

   <td width="628">impl From&lt;SqErr&gt; for UBaseErr{fn from(s:SqErr)-&gt;Self{UBaseErr::DbErr(s)}} impl From&lt;BcryptError&gt; for UBaseErr{fn from(b:BcryptError)-&gt;Self{UBaseErr::HashError(b)}}</td>

  </tr>

 </tbody>

</table>

<strong>Third</strong>, let’s implement some functions for the UserBase struct. We will start with the <em>add_user</em> method for adding a user to the database:

impl UserBase{   pub fn add_user(&amp;self, u_name:&amp;str, p_word:&amp;str)-&gt;Result&lt;(),UBaseErr&gt;{     let conn=sqlite::open(&amp;self.fname)?;     let hpass=bcrypt::hash(p_word,DEFAULT_COST)?;     let mut st= conn.prepare(“insert into    users(u_name, p_word) values (?,?);”)?;     st.bind(1,u_name)?;     st.bind(2,&amp;hpass as &amp;str)?;     st.next()?;

Ok(())

}

}

Now, can you write the pay function that inserts into the transaction table:

<table width="628">

 <tbody>

  <tr>

   <td width="628">pub fn pay(&amp;self, u_from:&amp;str, u_to:&amp;str, amount:i64)-&gt;Result&lt;(),UBaseErr&gt;{let conn=sqlite::open(&amp;self.fname)?;let mut st= conn.prepare(“insert into  transactions (u_from, u_to, t_date, t_amount)    values(?,?,datetime(”now”),?);”)?;st.bind(1,u_from)?;         st.bind(2,u_to)?;  st.bind(3,amount)?;st.next()?;Ok(())} </td>

  </tr>

 </tbody>

</table>

<h2>3- Writing Tests</h2>

<strong>Question 11: </strong>Write some test cases to test all the functions implemented for UserBase Struct. Your tests should ensure that your code runs properly, and the functions’ arguments are of correct types.