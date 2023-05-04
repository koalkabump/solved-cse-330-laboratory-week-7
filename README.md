Download Link: https://assignmentchef.com/product/solved-cse-330-laboratory-week-7
<br>
In this lab, you will be implemeting the ADT called Binary<u>Search</u>Tree. This data stucture will provide us with an alternative to vectors and linked lists when it comes to storing a list of data values.




<strong>Exercise 1:</strong> Implement the BinarySearchTree ADT  in a file BinarySearchTree.h exactly as shown below.  <em><u>As always, make an effort to copy mindfully, trying to understand the purpose of each line of</u> <u>code as you go  along.</u></em>







// BinarySearchTree.h

// afater Mark A. Weiss, Chapter 4




// KV replaced exceptions with assert statements;

// we are writing &lt;typename C&gt; to indicate that the template type must be // “comparable”, i.e., have defined &lt;, &gt; and == operators;




#ifndef BINARY_SEARCH_TREE_H

#define BINARY_SEARCH_TREE_H




#include &lt;cassert&gt; #include &lt;algorithm&gt; using namespace std;

template &lt;typename C&gt; class BinarySearchTree

{   public:

BinarySearchTree( ) : root{ nullptr }

{

}




BinarySearchTree( const BinarySearchTree &amp; rhs ) : root{ nullptr }

{

root = clone( rhs.root );

}




BinarySearchTree( BinarySearchTree &amp;&amp; rhs ) : root{ rhs.root }

{

rhs.root = nullptr;

}







~BinarySearchTree( )

{

makeEmpty();

}










BinarySearchTree &amp; operator=( const BinarySearchTree &amp; rhs )

{

BinarySearchTree copy = rhs;         std::swap( *this, copy );         return *this;




}







BinarySearchTree &amp; operator=( BinarySearchTree &amp;&amp; rhs )

{

std::swap( root, rhs.root );                return *this;

}




const C &amp; findMin( ) const

{

assert(!isEmpty());

return findMin( root )-&gt;element;

}




const C &amp; findMax( ) const

{        assert(!isEmpty());

return findMax( root )-&gt;element;

}      bool contains( const C &amp; x ) const

{

return contains( x, root );

}




bool isEmpty( ) const

{

return root == nullptr;

}

void printTree( ostream &amp; out = cout ) const

{

if( isEmpty( ) )

out &lt;&lt; “Empty tree” &lt;&lt; endl;         else

printTree( root, out );

}

void makeEmpty( )

{

makeEmpty( root );

}







void insert( const C &amp; x )

{

insert( x, root );

}




void insert( C &amp;&amp; x )

{

insert( std::move( x ), root );

}

void remove( const C &amp; x )

{

remove( x, root );

}

private:

struct BinaryNode

{

C element;

BinaryNode *left;

BinaryNode *right;




BinaryNode( const C &amp; theElement, BinaryNode *lt, BinaryNode *rt )

: element{ theElement }, left{ lt }, right{ rt } { }




BinaryNode( C &amp;&amp; theElement, BinaryNode *lt, BinaryNode *rt )

: element{ std::move( theElement ) }, left{ lt }, right{ rt } { }     };




BinaryNode *root;










// Internal method to insert into a subtree.

// x is the item to insert.

// t is the node that roots the subtree.

// Set the new root of the subtree.

void insert( const C &amp; x, BinaryNode * &amp; t )

{

if( t == nullptr )

t = new BinaryNode{ x, nullptr, nullptr };         else if( x &lt; t-&gt;element )             insert( x, t-&gt;left );         else if( t-&gt;element &lt; x )             insert( x, t-&gt;right );         else

;  // Duplicate; do nothing

}
















// Internal method to insert into a subtree.

// x is the item to insert.

// t is the node that roots the subtree.

// Set the new root of the subtree.

void insert( C &amp;&amp; x, BinaryNode * &amp; t )

{

if( t == nullptr )

t = new BinaryNode{ std::move( x ), nullptr, nullptr };         else if( x &lt; t-&gt;element )             insert( std::move( x ), t-&gt;left );         else if( t-&gt;element &lt; x )             insert( std::move( x ), t-&gt;right );         else

;  // Duplicate; do nothing

}







// Internal method to remove from a subtree.

// x is the item to remove.

// t is the node that roots the subtree.

// Set the new root of the subtree.

void remove( const C &amp; x, BinaryNode * &amp; t )

{

if( t == nullptr )

return;   // Item not found; do nothing         if( x &lt; t-&gt;element )             remove( x, t-&gt;left );         else if( t-&gt;element &lt; x )             remove( x, t-&gt;right );

else if( t-&gt;left != nullptr &amp;&amp; t-&gt;right != nullptr ) // Two children

{

t-&gt;element = findMin( t-&gt;right )-&gt;element;             remove( t-&gt;element, t-&gt;right );

}         else

{

BinaryNode *oldNode = t;

t = ( t-&gt;left != nullptr ) ? t-&gt;left : t-&gt;right;             delete oldNode;

}

}




// Internal method to find the smallest item in a subtree t.

// Return node containing the smallest item.

BinaryNode * findMin( BinaryNode *t ) const

{

if( t == nullptr )             return nullptr;         if( t-&gt;left == nullptr )             return t;

return findMin( t-&gt;left );

}







// Internal method to find the largest item in a subtree t.     // Return node containing the largest item.

BinaryNode * findMax( BinaryNode *t ) const

{

if( t != nullptr )             while( t-&gt;right != nullptr )                 t = t-&gt;right;         return t;

}




// Internal method to test if an item is in a subtree.

// x is item to search for.

// t is the node that roots the subtree.




bool contains( const C &amp; x, BinaryNode *t ) const

{

if( t == nullptr )             return false;         else if( x &lt; t-&gt;element )             return contains( x, t-&gt;left );         else if( t-&gt;element &lt; x )             return contains( x, t-&gt;right );         else

return true;    // Match

}      void makeEmpty( BinaryNode * &amp; t )

{

if( t != nullptr )

{

makeEmpty( t-&gt;left );             makeEmpty( t-&gt;right );             delete t;

}

t = nullptr;

}

void printTree( BinaryNode *t, ostream &amp; out ) const

{

if( t != nullptr )

{

printTree( t-&gt;left, out );             out &lt;&lt; t-&gt;element &lt;&lt; endl;             printTree( t-&gt;right, out );

}

}




BinaryNode * clone( BinaryNode *t ) const

{

if( t == nullptr )             return nullptr;         else

return new BinaryNode{ t-&gt;element,

clone( t-&gt;left ), clone( t-&gt;right ) };

}

}; #endif




<strong>Exercise  2:</strong> Program your own file BinarySearchTreeMain.cpp in which your main() function will test the new data structure. Declare an instance of BinarySearchTree (short:  BST) suitable to hold integer values. Then enter a random sequence of 25 integer values into the data structure (your values should NOT be in sorted order).




Use the print_Tree () member function in order to print out the values of the BST structure – <u>What do</u> <u>you notice?</u>




Next, remove 5 values from your BST and save them in a vector (use your own Vector.h or STL &lt;vector&gt;).  Print out the reduced BST.




<strong>Exercise 3:</strong> Next add the following member function do your BinarySearchTree class:




Under public:




void printInternal()

{

print_Internal(root,0);

}




Under private:




void printInternal(BinaryNode* t, int offset)

{

if (t == nullptr)        return;

for(int i = 1; i ≤ offset; i++)         cout  “..”    cout  t-&gt;element  endl;    printInternal(t-&gt;left, offset + 1);    printInternal(t-&gt;right, offset + 1);

}




Go back to your program BinarySearchTreeMain.cpp  and  change printTree to printInternal.

Compile and run your program, and see what you get.




Next, insert the 5 value that have been removed back into the BST.  Print the new BST with printInternal. <u>How does this printed  BST compare with the BST that the program printed before the removal of 5</u> <u>values?</u>  Same? Different? Explanation?




<strong>Credit for this lab:  </strong>(1) Sign up on the signup sheet. (2) Portal submission of your best effort for Exercises 1 and 2, 3 optional. Wednesday group submits by Friday 11:59pm; Monday group submits by Wednesday 11:59pm.