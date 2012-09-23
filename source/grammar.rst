Formal Grammar
==============

Syntax versus Semantics
-----------------------

This OpenSMARTS specification is divided into two distinct parts: A syntactic
specification specifies how the atoms, bonds, parentheses, digits and so forth
are represented, and a semantic specification that describes how those symbols
are interpreted as a sensible molecule. For example, the syntax specifies how
ring-closure digits are written, but the semantics require that they come in
pairs. Likewise, the syntax specifies how aromatic elements are written, but 
the semantics determines whether a particular ring system is actually aromatic.

For this specification, the syntax and semantics are explained separately; in
practice, the syntax and semantics are usually mixed together in the code that
implements a SMARTS parser. This chapter is only concerned with syntax.

.. _grammar:

Grammar
-------

The notations used in the formal grammar definitions are simple and should be
familiar to most. For completeness, they are repeated in the table below.

+----------------------------------------------------------------------------------+
| Notation                                                                         |
+==================================================================================+ 
| **TYPES**                                                                        |
+------------------------------------------+---------------------------------------+
| identifier                               | name for definition                   |
+------------------------------------------+---------------------------------------+
| '...'                                    | literal string                        |
+------------------------------------------+---------------------------------------+
| expr                                     | expression (may contain operators)    |
+------------------------------------------+---------------------------------------+
| **STATEMENTS**                                                                   |
+------------------------------------------+---------------------------------------+
| identifier ::= definition                | complex expressions (using operators) |
+------------------------------------------+---------------------------------------+
| **OPERATORS**                                                                    |
+------------------------------------------+---------------------------------------+
| expr expr                                | concatination is implicit             |
+------------------------------------------+---------------------------------------+
| expr \| expr                             | logical or                            |
+------------------------------------------+---------------------------------------+
| expr?                                    | none or one (0, 1)                    |
+------------------------------------------+---------------------------------------+
| expr+                                    | one or more (1, 2, 3 ...)             |
+------------------------------------------+---------------------------------------+
| expr*                                    | none or more (0, 1, 2, 3, ...)        |
+------------------------------------------+---------------------------------------+
| **SPECIAL**                                                                      |
+------------------------------------------+---------------------------------------+
| <identifier>                             | special character or implementation   |
|                                          | dependant event                       |
+------------------------------------------+---------------------------------------+

The actual SMARTS syntax and grammar is defined in the following table. Any
SMARTS that does not strictly adhere to these definitions is not completely
OpenSMARTS compliant but implementers may prefer to be less strict to allow
certain patterns that they already have or SMARTS from other sources and
toolkits which are not OpenSMARTS compliant. A common case for not strictly
accepting syntacticly valid SMARTS is that many implementations provide
extensions. Ideally a parser should have a **strict mode** and optionally
allow for extensions. The only difference between the OpenSMILES and
OpenSMARTS grammar is the definition of *bracket_atom* and *bond* is
replaced by *bond_expression*.

.. only:: html

  +--------------------------+-----------------------------------------------------------------------------------------+
  | Section                  | Formal Grammar                                                                          |
  +==========================+=========================================================================================+
  | **PRIMITIVES**                                                                                                     |
  +--------------------------+-----------------------------------------------------------------------------------------+
  |                          | *DIGIT* ::= '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'                   |
  |                          |                                                                                         |
  |                          | *NUMBER* ::= DIGIT+                                                                     |
  |                          |                                                                                         |
  |                          | *SPACE* ::= ' '                                                                         |
  |                          |                                                                                         |
  |                          | *TAB* ::= '\\t' | <ASCII: HT 0x09>                                                      |
  |                          |                                                                                         |
  |                          | *LINEFEED* := '\\n' | <ASCII: LF 0X0A>                                                  |
  |                          |                                                                                         |
  |                          | *CARIAGE_RETURN* ::= <ASCII: CR 0x0D>                                                   |
  |                          |                                                                                         |
  |                          | *END_OF_STRING* ::= <ASCII: EOT 0x04>                                                   |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | **VALID CHARACTERS**                                                                                               |
  +--------------------------+-----------------------------------------------------------------------------------------+
  |                          | *valid_char* ::= 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'G' | 'H' | 'I'                    |
  |                          |   | \| 'K' | 'L' | 'M' | 'N' | 'O' | 'P' | 'R' | 'S' | 'T' | 'U' | 'V'                  |
  |                          |   | \| 'W' | 'X' | 'Y' | 'Z'                                                            |
  |                          |   | \| 'a' | 'b' | 'c' | 'd' | 'e' | 'f' | 'g' | 'h' | 'i' | 'k' | 'l'                  |
  |                          |   | \| 'm' | 'n' | 'o' | 'p' | 'r' | 's' | 't' | 'u' | 'v' | 'x' | 'y'                  |
  |                          |   | \| 'z'                                                                              |
  |                          |   | \| '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'                        |
  |                          |   | \| '*' | '&' | ';' | ',' | '!' | '-' | '=' | '#' | '$' | ':' | '~'                  |
  |                          |   | \| '@' | '/' | '\\' | '+' | '-' | '(' | ')' | '[' | ']'                             |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | **ATOMS**                                                                                                          |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | :ref:`inatoms`           | *atom* ::= bracket_atom | aliphatic_organic | aromatic_organic | '*'                    |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | **ORGANIC SUBSET ATOMS**                                                                                           |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | :ref:`orgsbst`           | *aliphatic_organic* ::= 'B' | 'C' | 'N' | 'O' | 'S' | 'P' | 'F' | 'Cl' | 'Br' | 'I'     |
  |                          |                                                                                         |
  |                          | *aromatic_organic* ::= 'b' | 'c' | 'n' | 'o' | 's' | 'p'                                |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | **BRACKET ATOMS**                                                                                                  |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | :ref:`brcktatom`         | *bracket_atom* ::= '[' atom_expression+ ']'                                             |
  |                          |                                                                                         |
  |                          | *atom_expression* ::= atom_primitive                                                    |
  |                          |   | \| recursive_smarts\n                                                               |
  |                          |   | \| unary_operator atom_primitive                                                    |
  |                          |   | \| atom_expression atom_primitive                                                   |
  |                          |   | \| atom_expression binary_operator atom_primitive                                   |
  |                          |                                                                                         |
  |                          | *recursive_smarts* ::= '$(' chain ')'                                                   |
  |                          |                                                                                         |
  |                          | *unary_operator* ::= '!'                                                                |
  |                          |                                                                                         |
  |                          | *binary_operator* ::= '&' | ';' | ','                                                   |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | **ATOM PRIMITIVES**                                                                                                |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | :ref:`brcktatom`         | *atom_primitive* ::= isotope | symbol | atomic_number | 'a' | 'A'                       |
  |                          |   | \| degree | valence | connectivity                                                  |
  |                          |   | \| total_hcount | implicit_hcount                                                   |
  |                          |   | \| ring_membership | ring_size | ring_connectivity                                  |
  |                          |   | \| charge | chiral | class                                                          |
  |                          |                                                                                         |
  |                          | *symbol* ::= element_symbols | aromatic_symbols | '*'                                   |
  |                          |                                                                                         |
  |                          | *element_symbols* ::= 'H' | 'He' | 'Li' | 'Be' | 'B' | 'C' | 'N' | 'O' | 'F'            |
  |                          |   | \| 'Ne' | 'Na' | 'Mg' | 'Al' | 'Si' | 'P' | 'S' | 'Cl' | 'Ar' | 'K'                 |
  |                          |   | \| 'Ca' | 'Sc' | 'Ti' | 'V' | 'Cr' | 'Mn' | 'Fe' | 'Co' | 'Ni' | 'Cu'               |
  |                          |   | \| 'Zn' | 'Ga' | 'Ge' | 'As' | 'Se' | 'Br' | 'Kr' | 'Rb' | 'Sr' | 'Y'               |
  |                          |   | \| 'Zr' | 'Nb' | 'Mo' | 'Tc' | 'Ru' | 'Rh' | 'Pd' | 'Ag' | 'Cd' | 'In'              |
  |                          |   | \| 'Sn' | 'Sb' | 'Te' | 'I' | 'Xe' | 'Cs' | 'Ba' | 'Hf' | 'Ta' | 'W'                |
  |                          |   | \| 'Re' | 'Os' | 'Ir' | 'Pt' | 'Au' | 'Hg' | 'Tl' | 'Pb' | 'Bi' | 'Po'              |
  |                          |   | \| 'At' | 'Rn' | 'Fr' | 'Ra' | 'Rf' | 'Db' | 'Sg' | 'Bh' | 'Hs' | 'Mt'              |
  |                          |   | \| 'Ds' | 'Rg' | 'La' | 'Ce' | 'Pr' | 'Nd' | 'Pm' | 'Sm' | 'Eu' | 'Gd'              |
  |                          |   | \| 'Tb' | 'Dy' | 'Ho' | 'Er' | 'Tm' | 'Yb' | 'Lu' | 'Ac' | 'Th' | 'Pa'              |
  |                          |   | \| 'U' | 'Np' | 'Pu' | 'Am' | 'Cm' | 'Bk' | 'Cf' | 'Es' | 'Fm' | 'Md'               |
  |                          |   | \| 'No' | 'Lr'                                                                      |
  |                          |                                                                                         |
  |                          | *aromatic_symbols* ::= 'c' | 'n' | 'o' | 'p' | 's' | 'se' | 'as'                        |
  |                          |                                                                                         |
  |                          | *isotope* ::= NUMBER                                                                    |
  |                          |                                                                                         |
  |                          | *atomic_number* ::= '#' NUMBER                                                          |
  |                          |                                                                                         |
  |                          | *degree* ::= 'D' | 'D' NUMBER                                                           |
  |                          |                                                                                         |
  |                          | *valence* ::= 'v' | 'v' NUMBER                                                          |
  |                          |                                                                                         |
  |                          | *connectivity* ::= 'X' | 'X' NUMBER                                                     |
  |                          |                                                                                         |
  |                          | *total_hcount* ::= 'H' | 'H' DIGIT                                                      |
  |                          |                                                                                         |
  |                          | *implicit_hcount* ::= 'h' | 'h' DIGIT                                                   |
  |                          |                                                                                         |
  |                          | *ring_membership* ::= 'R' | 'R' NUMBER                                                  |
  |                          |                                                                                         |
  |                          | *ring_size* ::= 'r' | 'r' NUMBER                                                        |
  |                          |                                                                                         |
  |                          | *ring_connectivity* ::= 'x' | 'x' NUMBER                                                |
  |                          |                                                                                         |
  |                          | *charge* ::= '-' | '-' DIGIT | '+' | '+' DIGIT                                          |
  |                          |   | \| '--' *deprecated*                                                                |
  |                          |   | \| '++' *deprecated*                                                                |
  |                          |                                                                                         |
  |                          | *chiral* ::= '@' | '@@'                                                                 |
  |                          |   | \| '\@TH1' | '\@TH2'                                                                |
  |                          |   | \| '\@AL1' | '\@AL2'                                                                |
  |                          |   | \| '\@SP1' | '\@SP2' | '\@SP3'                                                      |
  |                          |   | \| '\@TB1' | '\@TB2' | '\@TB3' | ... | '\@TB19' | '\@TB20'                          |
  |                          |   | \| '\@OH1' | '\@OH2' | '\@OH3' | ... | '\@OH29' | '\@OH30'                          |
  |                          |                                                                                         |
  |                          | *class* ::= ':' NUMBER                                                                  |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | **BONDS**                                                                                                          |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | :ref:`inbonds`           | *bond_expression* ::= bond_primitive                                                    |
  |                          |   | \| unary_operator bond_primitive                                                    |
  |                          |   | \| bond_expression bond_primitive                                                   |
  |                          |   | \| bond_expression binary_operator bond_primitive                                   |
  |                          |                                                                                         |
  |                          | *bond_primitive* ::= '-' | '=' | '#' | '$' | ':' | '/' | '\' | '~' | '@'                |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | **CHAINS**                                                                                                         |
  +--------------------------+-----------------------------------------------------------------------------------------+
  |                          | *ringbond* ::= bond_expression? DIGIT | bond_expression? '%' DIGIT DIGIT                |
  |                          |                                                                                         |
  |                          | *branched_atom* ::= atom ringbond* branch*                                              |
  |                          |                                                                                         |
  |                          | *branch* ::= '(' chain ')' | '(' bond_expression chain ')' | '(' dot chain ')'          |
  |                          |                                                                                         |
  |                          | *chain* ::= branched_atom | chain branched_atom | chain bond_expression branched_atom   |
  |                          |   | \| chain dot branched_atom                                                          |
  |                          | *dot* ::= '.'                                                                           |
  +--------------------------+-----------------------------------------------------------------------------------------+
  | **SMARTS STRINGS**                                                                                                 |
  +--------------------------+-----------------------------------------------------------------------------------------+
  |                          | *smarts* ::= chain terminator                                                           |
  |                          |                                                                                         |
  |                          | *terminator* ::= SPACE | TAB | LINEFEED | CARRIAGE_RETURN | END_OF_STRING               |
  +--------------------------+-----------------------------------------------------------------------------------------+

.. only:: latex

  TODO: convert table...
