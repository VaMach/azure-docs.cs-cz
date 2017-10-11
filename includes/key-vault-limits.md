Klíč transakce (maximální počet povolených za 10 sekund, za transakcí trezoru každou oblast<sup>1</sup>):

|Typ klíče|Klíč HSM<br>Vytvoření klíče|Klíč HSM<br>Všechny ostatní transakce|Softwarový klíč<br>Vytvoření klíče|Softwarový klíč<br>Všechny ostatní transakce|
|:---|---:|---:|---:|---:|
|RSA 2048 bitů|5|1000|10|2000|
|RSA 3072 bitů|5|250|10|500|
|Šifrování RSA 4096 bitů|5|125|10|250|
|

Tajné klíče, spravovat klíče účtu úložiště a transakce úložiště:
| Typ transakce | Maximální počet povolených za 10 sekund, za transakcí trezoru podle oblastí<sup>1</sup> |
| --- | --- |
| Všechny transakce |2000 |
|

<sup>1</sup> existuje omezení celé předplatné pro všechny typy transakcí, který je 5 x za limit trezoru klíčů. Například HSM - dalších transakcí podle předplatného jsou omezeny na 5000 transakce za 10 sekund na jedno předplatné.
