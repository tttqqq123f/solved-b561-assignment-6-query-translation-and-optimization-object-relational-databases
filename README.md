Download Link: https://assignmentchef.com/product/solved-b561-assignment-6-query-translation-and-optimization-object-relational-databases
<br>
For this assignment you will need the material covered in the lectures on

Translating SQL to RA, RA query optimizations, and object-relational databases. For this assignment, you will need to submit 2 files:

<ol>

 <li>One such file is a .sql file that contains the SQL code relating to problems that requires such code.</li>

 <li>A second file with .pdf extension that contains your solutions for problems where RA expressions are requested. This .pdf file should also contain the answer to problems that require essay answers. No handwritten documents can be submitted.</li>

</ol>

<h1>1       Theoretical Problems</h1>

<ol>

 <li>In the translation algorithm from SQL to RA, when we eliminated set predicates, we tacitly assumed that the argument of each set predicate was a (possibly parameterized) SQL query that did not use a UNION, INTERSECT, or an EXCEPT operation.</li>

</ol>

In this problem, you are asked to extend the translation algorithm from SQL to RA such that (possibly parameterized) set predicates [NOT] IN are eliminated that have as an argument a SQL query that uses a UNION, INTERSECT, or EXCEPT operation.

More specifically, consider the following types of queries using the [NOT] IN set predicate.

SELECT L(r1,…,rk)

FROM             R1 r1, …, Rk rk

WHERE C1(r1,…,rk) AND

r1.A1 [NOT] IN (SELECT DISTINCT s1.B1

FROM            S1 s1,…, S1 sm

WHERE C2(s1,…,sm,r1,…,rk)

[UNION|INTERSECT|EXCEPT] SELECT DISTINCT t1.C1

FROM             T1 t1, …, Tn tn

WHERE C3(t1,…,tn,r1,…,rk))

Notice that there are six cases to consider:

<ul>

 <li>IN (… UNION …)</li>

 <li>IN (… INTERSECT …)</li>

 <li>IN (… EXCEPT …)</li>

 <li>NOT IN (… UNION …)</li>

 <li>NOT IN (… INTERSECT …)</li>

 <li>NOT IN (… EXCEPT …)</li>

</ul>

(a) Show how such SQL queries can be translated to equivalent RA expressions. Be careful in the translation since you should take into account that projections do not in general distribute over intersections or over set differences.

To get practice, first consider the following special case where <em>k </em>= 1, <em>m </em>= 1, and <em>n </em>= 1. I.e., the following case: <a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>

SELECT L(r1)

FROM       R1 r1

WHERE C1(r1) AND r1.A1 [NOT] IN (SELECT DISTINCT s1.B1

FROM       S1 s1

WHERE C2(s1,r1)

[UNION|INTERSECT|EXCEPT]

SELECT DISTINCT t1.C1

FROM       T1 t1

WHERE C3(t1,r1))

(b) Show how your translation can be improved when the variable “r1” does not occur in the conditions “C2” and “C3” in the subquery.

You don’t have to solve this query for the general case, but rather for the SQL query

SELECT L(r)

FROM       R1 r1

WHERE C1(r1) [NOT] IN r1.A (SELECT DISTINCT s1.B1

FROM       S1 s1

WHERE C2(s1)

[UNION|INTERSECT|EXCEPT]

SELECT DISTINCT t1.C1

FROM       T1 t1

WHERE C3(t1))

<ol start="2">

 <li>Let <em>R </em>be a relation with schema (<em>a,b,c</em>) and let <em>S </em>be a relation with schema (<em>a,b,d</em>). You may assume that the domains for the attributes <em>a</em>, <em>b</em>, and <em>c </em>are the same.</li>

</ol>

Prove the correctness of the following rewrite rule:

<em>π</em><em>a</em>(<em>R ./</em><em>R.a</em>=<em>S.b</em>∧<em>R.b</em>=<em>S.a </em><em>S</em>) = <em>π</em><em>a</em>(<em>π</em><em>a,b</em>(<em>R</em>) ∩ <em>π</em><em>b,a</em>(<em>S</em>))<em>.</em>

In other words, you have to prove that <em>π</em><em>a</em>(<em>R ./</em><em>R.a</em>=<em>S.b</em>∧<em>R.b</em>=<em>S.a </em><em>S</em>) ⊆ <em>π</em><em>a</em>(<em>π</em><em>a,b</em>(<em>R</em>) ∩ <em>π</em><em>b,a</em>(<em>S</em>)) and <em>π</em><em>a</em>(<em>R ./</em><em>R.a</em>=<em>S.b</em>∧<em>R.b</em>=<em>S.a </em><em>S</em>) ⊇ <em>π</em><em>a</em>(<em>π</em><em>a,b</em>(<em>R</em>) ∩ <em>π</em><em>b,a</em>(<em>S</em>))

<h1>2 Translating and Optimizing SQL Queries to Equivalent RA Expressions</h1>

Using the translation algorithm presented in class, translate each of the following SQL queries into equivalent RA expressions. For each query, provide its corresponding RA expression in your .pdf file. This RA expression needs to be formulated in the standard RA syntax.

Then use rewrite rules to optimize each of these RA expressions into an equivalent but optimized RA expression.

You are required to specify some, but not necessarily all, of the intermediate steps that you applied during the translations and optimizations. Use your own judgment to specify the most important steps.

During the optimization, take into account the primary keys and foreign key constraints that are assumed for the Student, Book, Buys, Major, and Cites relations.

<ol start="3">

 <li>“Find the sid and name of each student who majors in ‘CS’ and who bought a book that cites a higher priced book. ”</li>

</ol>

select s.sid, s.sname from student s where s.sid in (select m.sid from major m where m.major = ’CS’) and exists (select 1 from cites c, book b1, book b2 where (s.sid,c.bookno) in (select t.sid, t.bookno from buys t) and

c.bookno = b1.bookno and c.citedbookno = b2.bookno and b1.price &lt; b2.price);

<ul>

 <li>Using the translation algorithm presented in class, translate this SQL into and equivalent RA expression formulated in the standard RA syntax. Specify this RA expression in your .pdf file.</li>

 <li>Use the optimization rewrite rules, including those that rely on constraints, to optimize this RA expression. Specify this optimized RA expression in your .pdf file.</li>

</ul>

<ol start="4">

 <li>Find the sid, name, and major of each student who

  <ul>

   <li>does not major in ‘CS’,</li>

   <li>did not buy a book that less than $30,</li>

   <li>bought some book(s) that cost less than less than $50.</li>

  </ul></li>

</ol>

select distinct s.sid, s.sname, m.major from      student s, major m where s.sid = m.sid and s.sid not in (select m.sid from major m where m.major = ’CS’) and

s.sid &lt;&gt; ALL (select t.sid

from         buys t, book b

where t.bookno = b.bookno and b.price &lt; 30) and

s.sid in (select t.sid from    buys t, book b where t.bookno = b.bookno and b.price &lt; 60);

<ul>

 <li>Using the translation algorithm presented in class, translate this SQL into and equivalent RA expression formulated in the standard RA syntax. Specify this RA expression in your .pdf file.</li>

 <li>Use the optimization rewrite rules, including those that rely on constraints, to optimize this RA expression. Specify this optimized RA expression in your .pdf file.</li>

</ul>

<ol start="5">

 <li>Find each (<em>s,n,b</em>) triple where <em>s </em>is the sid of a student, <em>n </em>is the name of this student, and where <em>b </em>is the bookno of a book whose price is the most expensive among the books bought by that student.</li>

</ol>

select distinct s.sid, s.sname, b.bookno from    student s, buys t, book b where s.sid = t.sid and t.bookno = b.bookno and

b.price &gt;= ALL (select b.price

from       book b

where (s.sid,b.bookno) in (select t.sid, t.bookno from buys t));

<ul>

 <li>Using the translation algorithm presented in class, translate this SQL into and equivalent RA expression formulated in the standard RA syntax. Specify this RA expression in your .pdf file.</li>

 <li>Use the optimization rewrite rules, including those that rely on constraints, to optimize this RA expression. Specify this optimized RA expression in your .pdf file.</li>

</ul>

<ol start="6">

 <li>Find the bookno and title of each book that is not bought by all students who major in both ‘CS’ or in ‘Math’.</li>

</ol>

select b.bookno, b.title from           book b

where exists (select s.sid from         student s where s.sid in (select m.sid from major m

where m.major = ’CS’ UNION select m.sid from major m where m.major = ’Math’) and

s.sid not in (select t.sid from  buys t

where t.bookno = b.bookno));

<ul>

 <li>Using the translation algorithm presented in class, translate this SQL into and equivalent RA expression formulated in the standard RA syntax. Specify this RA expression in your .pdf file.</li>

 <li>Use the optimization rewrite rules, including those that rely on constraints, to optimize this RA expression. Specify this optimized RA expression in your .pdf file.</li>

</ul>

<strong>3        Experiments to Test the Effectiveness of Query</strong>

<h1>Optimization</h1>

In the following problems, you will conduct experiments to gain insight into whether or not query optimization can be effective. In other words, can it be determined experimentally if optimizing an SQL or an RA expression improves the time (and space) complexity of query evaluation?

You will need to use the PostgreSQL system to do you experiments. Recall that in SQL you can specify RA expression in a way that mimics it faithfully.

As part of the experiment, you might notice that PostgreSQL’s query optimizer does not fully exploit all the optimization that is possible as discussed in Lecture 14.

In the following problems you will need to generate artificial data of increasing size and measure the time of evaluating non-optimized and optimized queries. The size of this data can be in the ten or hundreds of thousands of tuples. This is necessary because on very small data is it is not possible to gain sufficient insights into the quality (or lack of quality) of optimization.

Consider a binary relation R(aint<em>,</em>bint). You can think of this relation as a graph, wherein each pair (<em>a,b</em>) represents and edge from <em>a </em>to <em>b</em>. (We work with directed graph. In other words edges (<em>a,b</em>) and (<em>b,a</em>) represent two different edges.) It is possible that R contains self-loops, i.e., edges of the form (<em>a,a</em>).

Besides the relation <em>R </em>we will also use a unary relation S(bint).

Along with this assignment, I have provided the code of two functions makerandomR(minteger<em>,</em>ninteger<em>,</em>linteger)

and makerandomS(nint<em>,</em>lint)

create or replace function makerandomR(m integer, n integer, l integer) returns void as $$ declare i integer; j integer; begin drop table if exists Ra; drop table if exists Rb; drop table if exists R;

create table Ra(a int); create table Rb(b int); create table R(a int, b int);

for i in 1..m loop insert into Ra values(i); end loop; for j in 1..n loop insert into Rb values(j); end loop; insert into R select * from Ra a, Rb b order by random() limit(l);

end;

$$ LANGUAGE plpgsql;

create or replace function makerandomS(n integer, l integer) returns void as $$ declare i integer; begin drop table if exists Sb; drop table if exists S; create table Sb(b int); create table S(b int);

for i in 1..n loop insert into Sb values(i); end loop; insert into S select * from Sb order by random() limit (l); end;

$$ LANGUAGE plpgsql;

When you run makerandomR(m,n,l);

for some values <em>m</em>, <em>n</em>, and <em>k</em>, this function will generate a random relation instance for <em>R </em>with <em>l </em>tuples that is subset of [1<em>,m</em>] × [1<em>,n</em>]. For example, makerandomR(3,3,4); might generate the relation instance

R

a     b

<ul>

 <li>1</li>

 <li>3</li>

 <li>3</li>

 <li>1</li>

</ul>

But, when you call makerandomR(3,3,4) again, it may now generate a different random relation such as

R

a     b

<ul>

 <li>2</li>

 <li>3</li>

 <li>1</li>

</ul>

1     1

Notice that when you call makerandomR(1000,1000,1000000)

it will make the entire relation [1<em>,</em>1000] × [1<em>,</em>1000] consisting of one million tuples.

The function makerandomS(n<em>,</em>l) will generate a random set of size <em>l </em>that is a subset of [1<em>,n</em>].

Now consider the following simple query <em>Q</em><sub>1</sub>:

select distinct r1.a

from           R r1, R r2

where r1.b = r2.a;

This query can be translated and optimized to the query <em>Q</em><sub>2</sub>:

select distinct r1.a

from                            R r1 natural join (select distinct r2.a as b from R r2) r2;

Image that you have created a relation R using the function makerandomR. Then when you execute in PostgreSQL the following

explain analyze select distinct r1.a

from           R r1, R r2

where r1.b = r2.a;

the system will return its execution plan as well as the execution time to evaluate <em>Q</em><sub>1 </sub>measured in ms.

And, when you execute in PostgeSQL the following

select distinct r1.a

from                            R r1 natural join (select distinct r2.a as b from R r2) r2;

the system will return its execution plan as well as the execution time to evaluate <em>Q</em><sub>2 </sub>measured in ms.

This permits us to compare the performance of the non-optimized query <em>Q</em><sub>1 </sub>with the optimized <em>Q</em><sub>2 </sub>for various-sized relation <em>R</em>.

Here are some of these comparisons for various different random relations R.

makerandomR                 <em>Q</em><sub>1 </sub>(in ms)        <em>Q</em><sub>2 </sub>(in ms)

<table width="262">

 <tbody>

  <tr>

   <td width="133">(100,100,1000)</td>

   <td width="79">4.9</td>

   <td width="50">1.5</td>

  </tr>

  <tr>

   <td width="133">(500,500,25000)</td>

   <td width="79">320.9</td>

   <td width="50">28.2</td>

  </tr>

  <tr>

   <td width="133">(1000,1000,100000)</td>

   <td width="79">2648.3</td>

   <td width="50">76.1</td>

  </tr>

  <tr>

   <td width="133">(2000,2000,400000)</td>

   <td width="79">23143.4</td>

   <td width="50">322.0</td>

  </tr>

  <tr>

   <td width="133">(5000,5000,2500000)</td>

   <td width="79">−−</td>

   <td width="50">1985.8</td>

  </tr>

 </tbody>

</table>

The “−−” symbol indicates that I had to stop the experiment because it was taken too long. (All the experiments where done on a MacBook pro.) Notice the significant difference between the execution times of the nonoptimized query <em>Q</em><sub>1 </sub>and the optimized query <em>Q</em><sub>2</sub>. So clearly, optimization works on query <em>Q</em><sub>1</sub>.

If you look at the query plan of PostgreSQL for <em>Q</em><sub>1</sub>, you will notice that it does a double nested loop and it therefore is <em>O</em>(|<em>R</em>|<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>) whereas for query <em>Q</em><sub>2 </sub>it runs in <em>O</em>(|<em>R</em>|). Clearly, optimization has helped significantly.<sup>2</sup>

<ol start="7">

 <li>Now consider query <em>Q</em><sub>3</sub>:</li>

</ol>

select distinct r1.a

from            R r1, R r2, R r3 where r1.b = r2.a and r2.b = r3.a;

<ul>

 <li>Translate and optimize this query and call it <em>Q</em><sub>4</sub>. Then write <em>Q</em><sub>4 </sub>as an SQL query with RA operations query just as was done for query <em>Q</em><sub>2</sub>.</li>

 <li>Compare queries <em>Q</em><sub>3 </sub>and <em>Q</em><sub>4 </sub>in a similar way as we did for <em>Q</em><sub>1 </sub>and <em>Q</em><sub>2</sub>.</li>

</ul>

You should experiment with different sizes for R. Incidentally, these relations do not need to use the same <em>m</em>,<em>n</em>, and <em>l </em>parameters as those shown in the above table for <em>Q</em><sub>1 </sub>and <em>Q</em><sub>2</sub>.

<ul>

 <li>What conclusions can you draw from the results of these experiments?</li>

</ul>

<ol start="8">

 <li>Now consider query <em>Q</em><sub>5 </sub>which is an implementation of the ONLY set semijoin between R and S. (See the lecture on set semijoins for more information.)</li>

</ol>

(Incidentally, if you look at the code for makerandomR you will see a relation Ra that provides the domain of all <em>a </em>values. You will need to use this relation in the queries. Analogously, in the code for makerandomS you will see the relation Sb that contains the domain of all <em>b </em>values.) In SQL, <em>Q</em><sub>5 </sub>can be expressed as follows:

select ra.a

from       Ra ra

where not exists (select r.b from     R r

where r.a = ra.a and

r.b not in (select s.b from S s));

<ul>

 <li>Translate and optimize this query and call it <em>Q</em><sub>6</sub>. Then write <em>Q</em><sub>6 </sub>as an SQL query with RA operations just as was done for <em>Q</em><sub>2 </sub></li>

 <li>Compare queries <em>Q</em><sub>5 </sub>and <em>Q</em><sub>6 </sub>in a similar way as we did for <em>Q</em><sub>1 </sub>and <em>Q</em><sub>2</sub>.</li>

</ul>

You should experiment with different sizes for R and S. (Vary the size of S from smaller to larger.) Also use the same value for the parameter <em>n </em>in makerandomR(m<em>,</em>n<em>,</em>l) and makerandomS(n<em>,</em>l) so that the maximum number of <em>b </em>values in R and S are the same.

<ul>

 <li>What conclusions can you draw from the results of these experiments?</li>

</ul>

<ol start="9">

 <li>Now consider query <em>Q</em><sub>7 </sub>which is an implementation of the ALL set semijoin between R and S. (See the lecture on set semijoins for more information.) In SQL, <em>Q</em><sub>7 </sub>can be expressed as follows:</li>

</ol>

select ra.a

from       Ra ra

where not exists (select s.b from     S s

where s.b not in (select r.b from  R r

where r.a = ra.a));

<ul>

 <li>Translate and optimize this query and call it <em>Q</em><sub>8</sub>. Then write <em>Q</em><sub>8 </sub>as an SQL query with RA operations just as was done for query <em>Q</em><sub>2 </sub></li>

 <li>Compare queries <em>Q</em><sub>7 </sub>and <em>Q</em><sub>8 </sub>in a similar way as we did for <em>Q</em><sub>1 </sub>and <em>Q</em><sub>2</sub>.</li>

</ul>

You should experiment with different sizes for R and S. (Vary the size of S from smaller to larger.) Also use the same value for the parameter <em>n </em>in makerandomR(m<em>,</em>n<em>,</em>l) and makerandomS(n<em>,</em>l) so that the maximum number of <em>b </em>values in R and S are the same.

<ul>

 <li>What conclusions can you draw from the results of these experiments?</li>

 <li>Furthermore, what conclusion can you draw when you compare you experiment with those for the ONLY set semijoin in problem 8?</li>

</ul>

<ol start="10">

 <li>Reconsider problem 8. We can also solve this problem by using a query wherein <em>set objects </em>are created (see Lecture 15). Below we show this query. Call this query <em>Q</em><sub>9</sub>.</li>

</ol>

Explain briefly how query <em>Q</em><sub>9 </sub>works and how it solves the problem.

with NestedR as (select r.a, array_agg(r.b order by 1) as Bs

from        R r

group by (r.a)),

SetS as (select array(select s.b from S s order by 1) as Bs) select r.a from NestedR r, SetS s where r.Bs &lt;@ s.Bs

union

select r.a from NestedR r where r.Bs = ’{}’;

<ul>

 <li>Now, using the same experimental data as in question 8, show the execution time of running this query and compare those with the ones obtained for queries <em>Q</em><sub>5 </sub>and <em>Q</em><sub>6</sub>.</li>

 <li>What conclusions can you draw from the results of these experiments? <strong>Solution</strong>:</li>

</ul>

<ol start="11">

 <li>Reconsider problem 9. We can also solve this problem by using a query wherein set objects are created. Below we show this query. Call this query <em>Q</em><sub>10</sub>. (We assume that <em>S </em>6= ∅.)</li>

</ol>

Explain briefly how query <em>Q</em><sub>10 </sub>works and how it solves the problem.

with NestedR as (select r.a, array_agg(r.b order by 1) as Bs

from         R r

group by (r.a)),

SetS as (select array(select s.b from S s order by 1) as Bs) select r.a from   NestedR r, SetS s where s.Bs &lt;@ r.Bs

<ul>

 <li>Now, using the same experimental data as in question 9, show the execution time of running this query and compare those with the ones obtained for queries <em>Q</em><sub>7 </sub>and <em>Q</em><sub>8</sub>.</li>

 <li>You should also run additional experiments that only compare query <em>Q</em><sub>8 </sub>and <em>Q</em><sub>10</sub>.</li>

 <li>What conclusions can you draw from the results of these experiments?</li>

</ul>

<h1>4      Formulating Queries in the Object-Relational Model</h1>

In the following problems, use the data provided for the student, majors, book, cites, buys relations.

The purpose of this assignment is to work with object-relational databases and to use these to solve queries.

<ol start="12">

 <li>Consider the function setunion which computes the set union of two sets represented as arrays. Notice that this function is defined polymorphically.</li>

</ol>

create or replace function setunion(A anyarray, B anyarray) returns anyarray as

$$ with

Aset as (select UNNEST(A)),

Bset as (select UNNEST(B)) select array( (select * from Aset) union (select * from Bset) order by 1); $$ language sql;

Actually, we can also write this functions as follows:

create or replace function setunion(A anyarray, B anyarray) returns anyarray as

$$ select array( select unnest(A) union select unnest(B) order by 1); $$ language sql;

<ul>

 <li>In the style of the setunion function, write a function setintersection that computes the intersection of two sets.</li>

 <li>In the style of the setunion function, write a function setdifference that computes the set difference of two sets.</li>

</ul>

You will need to use these functions in the remaining problems.

You can also make use of the function isIn which checks if an object <em>x </em>is in a set <em>S</em>. (Again this function is defined polymorphically.)

create or replace function isIn(x anyelement, S anyarray)

returns boolean as

$$ select x = SOME(S);

$$ language sql;

<ol start="13">

 <li>Consider the view student books(sid,books) which associates with each student the set of booknos of books he or she buys. Observe that there may be students who bought no books.

  <ul>

   <li>Define a view book students(bookno,students) which associates with each book the set of sids of students who bought that book. Observe that there may be books that are not bought by any student.</li>

   <li>Define a view book citedbooks(bookno,citedbooks) which associates with each bookno of a book the set of booknos of books cited by that book. Observe that there may be books that cite no books.</li>

   <li>Define a view book citingbooks(bookno,citingbooks) which associates with each bookno the set of booknos of books that cite that book. Observe that there may be books that are not cited.</li>

   <li>Define a view major students(major,students) which associates with each major the set of sids of students who have that major. (You can assume that each major has at least one student.)</li>

   <li>Define a view student majors(sid,majors) which associates with each student sid the set of his or her majors. Observe that there may be students who have no major.</li>

  </ul></li>

</ol>

Test that each of these views work properly. You will need to use them in the subsequent problems.

<ol start="14">

 <li>Using the above defined functions, views, and the book and student relations, specify the following queries in SQL.</li>

</ol>

So observe that you are not permitted to use the buys, cites, and major relations in your queries. You don’t need these relations since that are but encapsulated inside the functions. For example, a query such as

select distinct t.sid from  buys t, book b where t.bookno = b.bookno and b.price &lt; 50;

is <strong>not </strong>permitted. Rather, you should use the equivalent object-relational query

select distinct s.sid from student_books s, book b where isIn(b.bookno, s.books) and b.price &lt; 50;

<ul>

 <li>Find the bookno and title of each book that cites at least three books that cost less than $50.</li>

 <li>Find the bookno and title of each book that was not bought by any students who majors in ‘CS’.</li>

 <li>Find the sid of each student who bought all books that cost at least $50.</li>

 <li>Find the bookno of book that was not only bought by students who major in ‘CS’.</li>

 <li>Find the bookno and title of each book that was not only bought by students who bought all books that cost more than $45.</li>

 <li>Find sid-bookno pairs (<em>s,b</em>) such that not all books bought by student <em>s </em>are books that cite book <em>b</em>.</li>

 <li>Find the pairs (<em>b</em><sub>1</sub><em>,b</em><sub>2</sub>) of booknos of books that where bought by the same set of students.</li>

 <li>Find the pairs (<em>b</em><sub>1</sub><em>,b</em><sub>2</sub>) of booknos of books that where bought by the same number of students.</li>

 <li>Find the sid of each student who bought all but four books.</li>

 <li>Find the sid of each student who bought no more books than the combined number of books bought by the set of students who major in Psychology.</li>

</ul>

<a href="#_ftnref1" name="_ftn1">[1]</a> Once you can handle this case, the general case is a very similar.

<a href="#_ftnref2" name="_ftn2">[2]</a> It is actually really surprising that the PostgreSQL system did not optimize query <em>Q</em><sub>1 </sub>any better.