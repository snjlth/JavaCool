# Database


- Oracle - replace function
TRANSLATE('ABSDFSD', 'AB', '00'): 0-> original words, 1-> replace words, 2-> new words

E.g. Replace "AB" to "OO" below:
SELECT TRANSLATE('ABSDFSD', 'AB', '00') FROM DUAL;
OUTPUT: 00SDFSD