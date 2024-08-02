Micro-Changes for Conditionals

## Add Conjunct or Disjunct
  - **Description:** Modify an existing conditional statement by appending an additional condition using logical AND (`&&`) or logical OR (`||`) operators to refine or expand the criteria for the statement’s execution.
  - **Structure:**
    - `if ($c1)` → `if ($c1 && $c2)`
    - `if ($c1)` → `if ($c1 || $c2)`
  - **Motivation:** Attach extra conditions to refine or expand the criteria for the statement's execution with logical AND (`&&`) or logical OR (`||`) operators, respectively.
  - **Example:** any link?
  - **How to detect:** find an insertion of infix expression operator node within codition expression of "`&&`" or "`||`"

##  Wrap Statement in Block
  - **Description:** Enclose a single statement within curly braces following an if statement.
  - **Structure:** `if ($c) $stmt;` → `if ($c) { $stmt; }`
  - **Motivation:** 1) Prepare for adding new statements in the block. 2) Follow coding convention.
  - **How to detect:** find an insertion of a "Block", its parent node is "IfStatement". The before change "Then" part tree of that "IfStatement" is isomorphic to the "Block"'s only child tree.
  
## Adjust Condition Boundary
  - **Description:** Modify a comparison operator in a conditional statement to alter its boundary condition.
  - **Structure:**
    - `if ($n1 > $n2)` → `if ($n1 >= $n2)` 
    - `if ($n1 < $n2)` → `if ($n1 <= $n2)` 
    - `if ($n1 >= $n2)` → `if ($n1 > $n2)` 
    - `if ($n1 <= $n2)` → `if ($n1 > $n2)` 
  - **Motivation:** 1) Fix the boundary of a condition, 2) unify conditional branches or split one conditional branch to multiple
  - **How to detect:** find an update of a label of a node where "`>`" is changed to "`>=`" or "`<`" is changed to "`<=`", or vice versa.

## Flip Logic Operator 
  - **Description:** Alter the logical operator within a conditional statement, switching between AND (&&) and OR (||).
  - **Structure:**
    - `if ($c1 && $c2)` → `if ($c1 || $c2)`
    - `if ($c1 || $c2)` → `if ($c1 && $c2)`
  - **Motivation:** Fix?
  - **How to detect:** find an update of a label of a node where "`&&`" is changed to "`||`", or vice versa.

## Conditional To Boolean Return
  - **Description:** Simplify a conditional statement that directly returns a boolean value by replacing the if statement with a direct return of the condition’s evaluation.
  - **Structure:**
    - `if ($c) return true; else return false;` → `return $c;`
    - `if ($c) return false; else return true;` → `return !$c;`
  - **Motivation:** Simplification
  - **How to detect:** find a move of a tree, which is the 1st child of a "`IfStatement`". It is moved to be a child tree of a "`ReturnStatement`" node.

## Conditional to Switch
  - **Description:** Transform a series of if statements into a switch statement.
  - **Structure:** `if ($c1) { ... } else if ($c2) { ... }` → `switch ($v) { case $n1: ...; case $n2: ...; }`
  - **How to detect:** find a move of a tree, which is the 1st child of a "IfStatement". It is moved to be a child tree of a "SwitchStatement" node.

## Conditional to Expression
  - **Description:** Simplify an if-else statement into a concise ternary operation.
  - **Structure:** `if ($c) $v = $e1; else $v = $e2;` → `$x = $c ? $e1 : $e2;` 
  - **How to detect:** find a move of a tree, which is the descendant of a "IfStatement. It is moved to be a child tree of a ConditionalExpression" node.


## Wrap Statement in Conditional
  - **Description:** Wrap an existing statement, within an if statement
    - "which is not in a if-block": why this condition is needed for this concept? It can form a nested if  
  - **Structure:** `$stmt;` → `if ($c) $stmt;`
  - **How to detect:** We consider only a simple case where the target statement was not in any if conditional block.  Find a move of a tree, which is not the descendant of a "IfStatement" before change. After change, it is the descendat of a "IfStatement".


##  Extend Else with If
  - **Description:** Replace a simple else clause in an if-else statement with an else if condition, adding a new conditional check to the existing control flow for more specific action selection based on multiple conditions.
  - **Structure:** `if ($c1) { ...1 } else { ...2 }` → `if ($c) { ...1 } else if ($c2) { ... } else { ...2 }`
  - **How to detect:** find an insertion of a "IfStatement" node to be the child of another "IfStatement".


##  Extend If with Else
  - **Description:** Add an else clause to an existing if statement.
  - **Structure:** `if ($c) { ... }` → `if ($c) { ... } else { ... }`
  - **How to detect:** find an insertion of a node who is the 3rd child of a "`IfStatment`".

## Unwrap Statement from Conditional
  - **Description:** Remove the conditional check around a statement, so the statement executes unconditionally, independent of the previously specified condition.
  - **Structure:** `if ($c) $stmt;` → `$stmt;`
  - **How to detect:** find a move of a tree, which is originally a child tree of a "`IfStatement`" and is no longer a child tree of a "`IfStatement`" after the move. In addition, that  "`IfStatement`" is deleted.


## Add Conditional Statement 
  - **Description:** Add a new conditional block.
  - **Structure:** ` ` → `if ($c) { ... }`
  - **How to detect:** find an insertion where a "`IfStatement`" is inserted.

##  Move inward Condition
  - **Description:** Reorganize nested conditional statements by moving one of the conditions from an outer if statement to be combined with the condition of an inner if statement.
  - **Structure:** `if ($c1 && $c2) { if ($c3) { ... } }` → `if ($c1) { if ($c2 && $c3) { ... } }`
  - **How to detect:** find a move of a tree which is originally in the condition expression of the outer if statement, and it is moved into the condition expression of the inner if statement

##  Move outward Condition
  - **Description:** Reorganize nested conditional statements by moving one of the conditions from an inner if statement to be combined with the condition of an outer if statement.
  - **Structure:** `if ($c1) { if ($c2 && $c3) { ... } }` → `if ($c1 && $c2) { if ($c3) { ... } }`
  - **How to detect:** find a move of a tree which is originally in the condition expression of the inner if statement, and it is moved in to the condition expression of the outer if statement.

## Remove Conditional Statement
  - **Description:** Remove an existing conditional block.
  - **Structure:** `if ($c) { ... }` → ``
  - **How to detect:** find a deletion of a tree which is a "IfStatement".

## Unwrap Statement from Block
  - **Description:** Remove the curly braces from a block containing a single statement following an if statement.
  - **Structure:** `if ($c) { $stmt; }` → `if ($c) $stmt;`
  - **How to detect:** find a move of a tree which is a "`Block`" or its parent is a "`Block`". The tree is the child of a "`IfStatement`". The condition expressions of that "`IfStatement`" is isomorphic to each other and so as the "`IfStatement`"'s 2nd child nodes.

##  Remove Else
  - **Description:** Remove the else clause from an if-else statement, leaving only the if statement and its associated action.
  - **Structure:** `if ($c) { ... } else { ... }` → `if ($c) { ... }`
  - **How to detect:** find a deletion of a tree which is the 3rd child of a "IfStatement"

##  Reverse Condition
  - **Description:** Invert the logic of a conditional expression within an if statement, changing the condition to its logical opposite.
  - **Structure:** `if ($v1 == $v2)`→ `if ($v1 != $v2)`
  - **How to detect:**
    - 1. find a update of a label changing from "`!=`" to "`==`" or vice versa.
    - 2. find an insertion or deletion of the node "`PREFIX_EXPRESSION_OPERATOR: !`" to a conditional expression.
    - 3. find a update of a lable changing from "`<`" to "`>=`", or from "`<`" to "`>=`", or "`<=`" to "`>`", or "`>=`" to "`<`" (including changing from `a<b` -> `b<=a`, in another word, order changed) 

##  Swap Then and Else
  - **Description:** Swap the actions of the then and else clauses of an if-else statement.
  - **Structure:** `if ($c) { ...1 } else { ...2 }` → `if ($c) { ...2 } else { ...1 }`
  - **How to detect:** find a move of a tree which is the child of a "`IfStatement`" and it is moved to be the child of the same "`IfStatement`".

## Remove Conjunct or Disjunct
  - **Description:** Modify an existing conditional statement by removing conditions from a compound logical expression that concatenates multiple conditions by logical AND (`&&`) or logical OR (`||`)
  - **Structure:**
    - `if ($c1 && $c2)` → `if ($c1)`
    - `if ($c1 && $c2)` → `if ($c2)`
    - `if ($c1 || $c2)` → `if ($c1)`
    - `if ($c1 || $c2)` → `if ($c2)`
  - **How to detect:** find an deletion of infix expression operator node within codition expression of "`&&`" or "`||`"
