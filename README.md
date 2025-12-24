# Solveur de sudoku avec choco


Sudoku utilisant la programmation par contraintes et la bibliothèque Choco Solver pour résoudre des grilles de Sudoku de complexité variable.

Choco solver est une bibliothèque Java très puissante dédiée à la programmation par contraintes.
Plus d'informations [Ici](http://www.choco-solver.org/)

Un exemple pour résoudre un problème de sudoku à l'aide d'un ensemble de contraintes uniquement.

## première étape : lire une grille de sudoku 
 
```java
  String[] sudoku = new String[] {
				"**2**8***",
				"6*5*9418*",
				"*4*****95",
				"****1*629",
				"****2*357",
				"*****9***",
				"5**243***",
				"*2398*57*",
				"9*1*6*2*4"
  SudokuSolver grid = new SudokuSolver(sudoku);
  grid.show();
      
```
```
-------------
|  2|  8|   |
|6 5| 94|18 |
| 4 |   | 95|
-------------
|   | 1 |629|
|   | 2 |357|
|   |  9|   |
-------------
|5  |243|   |
| 23|98 |57 |
|9 1| 6 |2 4|
-------------
```

## Créer un modèle et quelques variables/constantes

```java
Model model = new Model("Sudoku solver");
...
// variable (à résoudre), un identifiant, de type entier compris entre 1 et 9
IntVar varToSolve = model.intVar("" + rowIdx + "" + colIdx, 1, 9);

// constante (nombres connus)
IntVar varConstant = model.intVar(num);
```

## Ajouter quelques contraintes

Contraintes sur toutes les lignes/colonnes et la grille interne, toutes les valeurs sont différentes.

```java
for ( int idx = 0; idx < SIZE; idx++) {
    // Ajouter des contraintes sur les lignes, toutes différentes...
    model.allDifferent(getRowVars(idx)).post();
    
    // Ajouter des contraintes sur les colonnes, toutes différentes...
    model.allDifferent(getColVars(idx)).post();
			
    // Ajouter des contraintes de grille interne, toutes différentes
    model.allDifferent(getInnerGridVars(idx)).post();
}
```

## Résolution

Il suffit maintenant d'appeler resolve sur le modèle.

```java
		if ( model.getSolver().solve() ) {
			show();
		}
		else {
			System.out.println("No solution found");
		}
```

```
-------------
|792|158|463|
|635|794|182|
|148|632|795|
-------------
|374|815|629|
|819|426|357|
|256|379|841|
-------------
|567|243|918|
|423|981|576|
|981|567|234|
-------------
```

C'etait juste pour un test

