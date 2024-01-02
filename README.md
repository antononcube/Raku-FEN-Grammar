## Forsyth–Edwards Notation (FEN) Grammar
[![SparrowCI](https://ci.sparrowhub.io/project/git-vushu-fen-grammar/badge?foo=bar)](https://ci.sparrowhub.io)
### Parsing FEN
```
use FEN::Grammar;
my $fen = 'rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR b KQkq e3 0 1';
my $match = FEN::Grammar.parse($fen);
```

State is optional, if given fen positions only it will still parse.
```
use FEN::Grammar;
my $fen = 'r1bk3r/p2pBpNp/n4n2/1p1NP2P/6P1/3P4/P1P1K3/q5b1'; # We only supply the positions.
my $match = FEN::Grammar.parse($fen);
```

### Using FEN::Actions
We can create an array of ranks describing the position.
```
use FEN::Grammar;
use FEN::Actions;

my $actions = FEN::Actions.new;
my $match = FEN::Grammar.parse($fen, actions => $actions);
my $result = $match.made; # returns a FEN::Result
```

### Transform to ASCII matrix
For easy usage or rendering we can use FEN::Utils.

```
use FEN::Utils;
# $result generated by FEN::Actions
my @matrix = FEN::Utils::to-matrix($result.ranks);

# display 
sink @matrix.map: *.say;

[r . b k . . . r]
[p . . p B p N p]
[n . . . . n . .]
[. p . N P . . P]
[. . . . . . P .]
[. . . P . . . .]
[P . P . K . . .]
[q . . . . . b .]
```
### Tranform to unicoded matrix
```
use FEN::Utils;
my @matrix = FEN::Utils::to-unicoded-matrix($result.ranks);

# display 
sink @matrix.map: *.say;

[♜ . ♝ ♚ . . . ♜]
[♟ . . ♟ ♗ ♟ ♘ ♟]
[♞ . . . . ♞ . .]
[. ♟ . ♘ ♙ . . ♙]
[. . . . . . ♙ .]
[. . . ♙ . . . .]
[♙ . ♙ . ♔ . . .]
[♛ . . . . . ♝ .]
```

### From ASCII matrix back to FEN
```
my $ascii-matrix = q:to/ASCII/;
[r . b k . . . r]
[p . . p B p N p]
[n . . . . n . .]
[. p . N P . . P]
[. . . . . . P .]
[. . . P . . . .]
[P . P . K . . .]
[q . . . . . b .]
ASCII

my $fen = FEN::Utils::ascii-to-fen($ascii-matrix);
say $fen;

r1bk3r/p2pBpNp/n4n2/1p1NP2P/6P1/3P4/P1P1K3/q5b1
```

### Display state
```
# state for given fen:rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR b KQkq e3 0 1
$result.show-state;

Active color: Black
White may castle on: kingside, queenside
Black may castle on: kingside, queenside
En-passant: e3
Halfmove clock: 0
Fullmove number: 1
```

### Position set
get positions as hash set from FEN::Result
```
say $result.position-set;

{a 1 => R, a 2 => P, a 3 => , a 4 => , a 5 => , a 6 => , a 7 => p, a 8 => r, b 1 => N, b 2 => P, b 3 => , b 4 => , b 5 => , b 6 => , b 7 => p, b 8 => n, c 1 => B, c 2 => P, c 3 => , c 4 => , c 5 => , c 6 => , c 7 => p, c 8 => b, d 1 => Q, d 2 => P, d 3 => , d 4 => , d 5 => , d 6 => , d 7 => p, d 8 => q, e 1 => K, e 2 => , e 3 => , e 4 => P, e 5 => , e 6 => , e 7 => p, e 8 => k, f 1 => B, f 2 => P, f 3 => , f 4 => , f 5 => , f 6 => , f 7 => p, f 8 => b, g 1 => N, g 2 => P, g 3 => , g 4 => , g 5 => , g 6 => , g 7 => p, g 8 => n, h 1 => R, h 2 => P, h 3 => , h 4 => , h 5 => , h 6 => , h 7 => p, h 8 => r}
```
