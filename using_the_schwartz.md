## Using the Schwartz: Part I

A useful idio…m.

Aspiring software developers might learn a lot about algorithms in school, but modern programming languages and libraries can do much of the implementation. There are, however, common and important tasks that show up regularly that are too small to implement as a library, but do require some knowledge of programming idioms and underlying concepts.

One of those recurring tasks is "sorting a list by a related value." For example, you might have the results of an exam as tuples of student names and exam grades. I'm going to demonstrate with some code in Perl, because I'm a crotchety yet lovable dinosaur (and I wrote a whole dang book about Perl), but you'll see translations into more modern languages later. Let's start with some 70s-flavored students and their corresponding grades:

my %exam_grade = qw(
  Bob 92
  Alice 88
  Ted 88
  Carol 95
);

Here I've initialized a hash with key-value pairs where the key is the name of the student (there is an assumption here that the names are unique), and their corresponding grade, which is not necessarily unique.

These key-value pairs of student names and corresponding grades are in no particular order. You might want the names sorted alphabetically. This is straightforward:
my @sorted_name = sort keys %exam_grade;

print "$_\n" for @sorted_name;

But you also might want them sorted in order of grade. This is also easy and idiomatic, using the the sortoperator's optional comparison block:
my @sorted_name = sort 
 { $exam_grade{$a} <=> $exam_grade{$b} }
 keys %exam_grade;
print "$_ $exam_grade{$_}\n" for @sorted_name;
Note: I have this habit from a coding standard long, long ago of using singular names for arrays, lists, hashes, and the like. Try it. You might think it makes sense!
What's happening here is that rather than use the student name itself (key) in the comparison in the sort, we use the corresponding grade (value) looked up in the hash. This gives us:
Alice 88
Ted 88
Bob 92
Carol 95
This is great, except that you might prefer these in descending order of grade. The idiom here is to swap the sides of the comparison:
my @sorted_name = sort
 { $exam_grade{$b} <=> $exam_grade{$a} }
 keys %exam_grade;
print "$_ $exam_grade{$_}\n" for @sorted_name;
Which yields:
Carol 95
Bob 92
Alice 88
Ted 88
