Micro-Changes for Conditionals

## Add Conjunct or Disjunct
  - **Description:** Modify an existing conditional statement by appending an additional condition using logical AND (`&&`) or logical OR (`||`) operators to refine or expand the criteria for the statement’s execution.
  - **Structure:**
    - `if ($c1)` → `if ($c1 && $c2)`
    - `if ($c1)` → `if ($c1 || $c2)`
  - **Motivation:** Attach extra conditions to refine or expand the criteria for the statement's execution with logical AND (`&&`) or logical OR (`||`) operators, respectively.
  - **Example:** [example](https://github.com/bennidi/mbassador/commit/e170301c2c591dc3289909f2d3ea2ab10e293753#diff-faefffce0483ec1ad8ad952bbcc783af5d6d5cf85502c880533b10b6feb0650dL40)
  - **How to detect:** find an insertion of infix expression operator node within codition expression of "`&&`" or "`||`"

##  Wrap Statement in Block
  - **Description:** Enclose a single statement within curly braces following an if statement.
  - **Structure:** `if ($c) $stmt;` → `if ($c) { $stmt; }`
  - **Motivation:** 1) Prepare for adding new statements in the block. 2) Follow coding convention.
  - **Example:** [example](https://github.com/checkstyle/checkstyle/commit/59af65d194da001d34a48da91ba296e6c5d439cb#diff-9154404f554819b5804288e394acc533f7a876baed904d559d6d0b88a3ee48a1L72
  )
  - **How to detect:** find an insertion of a "Block", its parent node is "IfStatement". The before change "Then" part tree of that "IfStatement" is isomorphic to the "Block"'s only child tree.
  
## Adjust Condition Boundary
  - **Description:** Modify a comparison operator in a conditional statement to alter its boundary condition.
  - **Structure:**
    - `if ($n1 > $n2)` → `if ($n1 >= $n2)` 
    - `if ($n1 < $n2)` → `if ($n1 <= $n2)` 
    - `if ($n1 >= $n2)` → `if ($n1 > $n2)` 
    - `if ($n1 <= $n2)` → `if ($n1 > $n2)` 
  - **Motivation:** 1) Fix the boundary of a condition, 2) unify conditional branches or split one conditional branch to multiple
  - **Example:**[example](https://github.com/jfinal/jfinal/commit/b7a4a0916d5df4aefffcb151c8e6ebcbe4b23564#diff-0b361deb0b884860e5441f5801344bff55e60bfdd65b782ba5c02b49d14c0f1bL186)
  - **How to detect:** find an update of a label of a node where "`>`" is changed to "`>=`" or "`<`" is changed to "`<=`", or vice versa.

## Flip Logic Operator 
  - **Description:** Alter the logical operator within a conditional statement, switching between AND (&&) and OR (||).
  - **Structure:**
    - `if ($c1 && $c2)` → `if ($c1 || $c2)`
    - `if ($c1 || $c2)` → `if ($c1 && $c2)`
  - **Motivation:** 
  - **Example:** [example](https://github.com/geoserver/geoserver/commit/a6703c557b4479bbb3b1e2ea89dff943d7951959#diff-dfd451584bd557ca66c41d1536f5fe326f16f3600be0f7f773e233f704041eb9L40)
  - **How to detect:** find an update of a label of a node where "`&&`" is changed to "`||`", or vice versa.

## Conditional To Boolean Return
  - **Description:** Simplify a conditional statement that directly returns a boolean value by replacing the if statement with a direct return of the condition’s evaluation.
  - **Structure:**
    - `if ($c) return true; else return false;` → `return $c;`
    - `if ($c) return false; else return true;` → `return !$c;`
  - **Motivation:** Simplification
  - **Example:**[example](https://github.com/checkstyle/checkstyle/commit/c09131defe36dde56b2d9767d1953d93e20bd200#diff-7c2aa1c035a9b7e7e8949140a0a51e71ac35f8dbc2e3a822e6991c2053606fdcL367)
  - **How to detect:** find a move of a tree, which is the 1st child of a "`IfStatement`". It is moved to be a child tree of a "`ReturnStatement`" node.

## Conditional to Switch
  - **Description:** Transform a series of if statements into a switch statement.
  - **Structure:** `if ($c1) { ... } else if ($c2) { ... }` → `switch ($v) { case $n1: ...; case $n2: ...; }`
  - **Example:** [example](https://github.com/checkstyle/checkstyle/commit/a00dd508d3d5995bea73441ced0b2b3233dc6679#diff-7764f183a8d83d16ede354181b80531d4c7bdb79d56f89c8fd3c92024118eba0L198
  )
  - **How to detect:** find a move of a tree, which is the 1st child of a "IfStatement". It is moved to be a child tree of a "SwitchStatement" node.

## Conditional to Expression
  - **Description:** Simplify an if-else statement into a concise ternary operation.
  - **Structure:** `if ($c) $v = $e1; else $v = $e2;` → `$x = $c ? $e1 : $e2;` 
  - **Example:**[example](https://github.com/jfinal/jfinal/commit/a8f090a26e94a67edac85d0a129ce099afea8db2#diff-6f3846c7cf4fc4dd2d0a38cb7b9b1bf749409f10ab54f397c9ff341f3ee84639L105)
  - [example](https://github.com/brettwooldridge/HikariCP/commit/3295443e9b4b804d06be1e8a48dd89f1278596fc#diff-07daab97a2bb5a7120702afad57061cd655fa631d5e66c94580c0385f08aae2dL351)
  - **How to detect:** find a move of a tree, which is the descendant of a "IfStatement. It is moved to be a child tree of a ConditionalExpression" node.


## Wrap Statement in Conditional
  - **Description:** Wrap an existing statement, within an if statement
    - "which is not in a if-block": why this condition is needed for this concept? It can form a nested if  
  - **Structure:** `$stmt;` → `if ($c) $stmt;`
  - **Example:** [example](https://github.com/bennidi/mbassador/commit/e4b6a84ab8d2487fe610d294535f3f593b2f819c#diff-6c69416cf9d1692cfd8366cf53351054edabec1980e798bf0784884ae39d7d7dL92
  )
  - **How to detect:** We consider only a simple case where the target statement was not in any if conditional block.  Find a move of a tree, which is not the descendant of a "IfStatement" before change. After change, it is the descendat of a "IfStatement".


##  Extend Else with If
  - **Description:** Replace a simple else clause in an if-else statement with an else if condition, adding a new conditional check to the existing control flow for more specific action selection based on multiple conditions.
  - **Structure:** `if ($c1) { ...1 } else { ...2 }` → `if ($c) { ...1 } else if ($c2) { ... } else { ...2 }`
  - **Example:** [example](https://github.com/jfinal/jfinal/commit/b7a4a0916d5df4aefffcb151c8e6ebcbe4b23564#diff-a380bc4b0ed8c4b195965ef4d6c52bee6d1020bcc2d931ef66be9e2f01c5103cL44)
  - **How to detect:** find an insertion of a "IfStatement" node to be the child of another "IfStatement".


##  Extend If with Else
  - **Description:** Add an else clause to an existing if statement.
  - **Structure:** `if ($c) { ... }` → `if ($c) { ... } else { ... }`
  - **Example:** [example](https://github.com/luontola/retrolambda/commit/5519d0cd3bbe17beda848dd28cc9c214e867674a#diff-2a6acf10bd46d7a8ef9ea549aaa78e13404a58e9e1dd45a9e274d50576a9c7abL41
  )
  - **How to detect:** find an insertion of a node who is the 3rd child of a "`IfStatment`".

## Unwrap Statement from Conditional
  - **Description:** Remove the conditional check around a statement, so the statement executes unconditionally, independent of the previously specified condition.
  - **Structure:** `if ($c) $stmt;` → `$stmt;`
  - **Example:** [example](https://github.com/brettwooldridge/HikariCP/commit/00b77f9cd3853848a0854560847f0f5c9953e700#diff-290114ea5dface33b2ccc1514b2a08554538ac6f4c3c2719a6586a77cf40e6bcL202
  )
  - **How to detect:** find a move of a tree, which is originally a child tree of a "`IfStatement`" and is no longer a child tree of a "`IfStatement`" after the move. In addition, that  "`IfStatement`" is deleted.


## Add Conditional Statement 
  - **Description:** Add a new conditional block.
  - **Structure:** ` ` → `if ($c) { ... }`
  - **Example:** [example](https://github.com/bennidi/mbassador/commit/7c0c0b6f825d98793525595a7be5ed43f6b99e43#diff-c68e9fa1ea223c0e020dba697f0e4d697ddf0dab8a5c3eb6b028267a216fe8baR48
  )
  - **How to detect:** find an insertion where a "`IfStatement`" is inserted.

##  Move inward Condition
  - **Description:** Reorganize nested conditional statements by moving one of the conditions from an outer if statement to be combined with the condition of an inner if statement.
  - **Structure:** `if ($c1 && $c2) { if ($c3) { ... } }` → `if ($c1) { if ($c2 && $c3) { ... } }`
  - **Example:** [example](https://github.com/checkstyle/checkstyle/commit/2db0dab62fc4986c3bebb38a1b8cac857f37664a#diff-b46089f9e674e8a77bd29549e21d47833e5a0522c17193feb514190710e14addL149)
  - **How to detect:** find a move of a tree which is originally in the condition expression of the outer if statement, and it is moved into the condition expression of the inner if statement

##  Move outward Condition
  - **Description:** Reorganize nested conditional statements by moving one of the conditions from an inner if statement to be combined with the condition of an outer if statement.
  - **Structure:** `if ($c1) { if ($c2 && $c3) { ... } }` → `if ($c1 && $c2) { if ($c3) { ... } }`
  - **Example:** [example](https://github.com/brettwooldridge/HikariCP/commit/4186e325b7efa3cd691781baea40e374bd1f9c71#diff-245fdd75a16523d164d1d2eed06438c1d11d68732602f23136db8fa09b48560eL193
  )
  - **How to detect:** find a move of a tree which is originally in the condition expression of the inner if statement, and it is moved in to the condition expression of the outer if statement.

## Remove Conditional Statement
  - **Description:** Remove an existing conditional block.
  - **Structure:** `if ($c) { ... }` → ``
  - **Example:** [example](https://github.com/bennidi/mbassador/commit/7c0c0b6f825d98793525595a7be5ed43f6b99e43#diff-ff2ac635bfc682a7561aa292ecd1f879b623ed6207783ac86d8f049801b58062L60
  )
  - **How to detect:** find a deletion of a tree which is a "IfStatement".

## Unwrap Statement from Block
  - **Description:** Remove the curly braces from a block containing a single statement following an if statement.
  - **Structure:** `if ($c) { $stmt; }` → `if ($c) $stmt;`
  - **Example:** [example](https://github.com/jfinal/jfinal/commit/e3838d76358a151922faefc556387abbd3db9369#diff-e493321fb4c7ab40c02c8d72081ee8bd8842ecf9066ddc7bcebcdd5b71b89e5bL88
  )
  - **How to detect:** find a move of a tree which is a "`Block`" or its parent is a "`Block`". The tree is the child of a "`IfStatement`". The condition expressions of that "`IfStatement`" is isomorphic to each other and so as the "`IfStatement`"'s 2nd child nodes.

##  Remove Else
  - **Description:** Remove the else clause from an if-else statement, leaving only the if statement and its associated action.
  - **Structure:** `if ($c) { ... } else { ... }` → `if ($c) { ... }`
  - **Example:** [example](https://github.com/checkstyle/checkstyle/commit/c3e5b6847935c79831965c774babd679c26ce0f2#diff-7c7dbe32f99f4c91fd7d9e7d41c5cb8dbbdde041679e78f17020c951b35554bbL384)
  - **How to detect:** find a deletion of a tree which is the 3rd child of a "IfStatement"

##  Reverse Condition
  - **Description:** Invert the logic of a conditional expression within an if statement, changing the condition to its logical opposite.
  - **Structure:** `if ($v1 == $v2)`→ `if ($v1 != $v2)`
  - **Example:** [example](https://github.com/jfinal/jfinal/commit/b7a4a0916d5df4aefffcb151c8e6ebcbe4b23564#diff-e493321fb4c7ab40c02c8d72081ee8bd8842ecf9066ddc7bcebcdd5b71b89e5bL50)
  - **How to detect:**
    - 1. find a update of a label changing from "`!=`" to "`==`" or vice versa.
    - 2. find an insertion or deletion of the node "`PREFIX_EXPRESSION_OPERATOR: !`" to a conditional expression.
    - 3. find a update of a lable changing from "`<`" to "`>=`", or from "`<`" to "`>=`", or "`<=`" to "`>`", or "`>=`" to "`<`" (including changing from `a<b` -> `b<=a`, in another word, order changed) 
  

##  Swap Then and Else
  - **Description:** Swap the actions of the then and else clauses of an if-else statement.
  - **Structure:** `if ($c) { ...1 } else { ...2 }` → `if ($c) { ...2 } else { ...1 }`
  - **Example:** [example](https://github.com/geoserver/geoserver/commit/4f04d65c4bef761d20666bf600ffb550d8010925#diff-e063633ff5ee5e8dd79e8535c395566bd772dac2a12fad2587abede70584ee64L403)
  - **How to detect:** find a move of a tree which is the child of a "`IfStatement`" and it is moved to be the child of the same "`IfStatement`".

## Remove Conjunct or Disjunct
  - **Description:** Modify an existing conditional statement by removing conditions from a compound logical expression that concatenates multiple conditions by logical AND (`&&`) or logical OR (`||`)
  - **Structure:**
    - `if ($c1 && $c2)` → `if ($c1)`
    - `if ($c1 && $c2)` → `if ($c2)`
    - `if ($c1 || $c2)` → `if ($c1)`
    - `if ($c1 || $c2)` → `if ($c2)`
  - **Example:** [example](https://github.com/geoserver/geoserver/commit/7580eaa5d20183aaa567fb3a93ae68a9b0796374#diff-4d7b280d3cfb78d1f2eecd336c8d77cda0492f85856e9729cdefcbb3dd1345b3L443)
  - **How to detect:** find an deletion of infix expression operator node within codition expression of "`&&`" or "`||`"
