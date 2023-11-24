:- [cd, aufg2_test, familie].

% *** AUFGABE 1 ***

% 1 - Es werden alle Stücke mit ihrer Tonart ausgegeben -LARISSA PYCHLAU
collect_stueck(Collection) :- findall((TITEL, TONART), (stueck(SNR, _KNR, TITEL, TONART, _OPUS), SNR\==null, TITEL\==null, TONART\==null), Ausgabe1), list_to_set(Ausgabe1, Collection).

% 2 - Es werden alle Stücke mit einer Spielzeit zwischen 88 und 342 ausgegeben -LARISSA PYCHLAU
collect_gs(Collection) :- findall((CDNR, GESAMTSPIELZEIT), (cd(CDNR, _NAME, _HERSTELLER, _ANZ_CDS, GESAMTSPIELZEIT), CDNR\==null, GESAMTSPIELZEIT\==null, GESAMTSPIELZEIT>=88, GESAMTSPIELZEIT=<342), Ausgabe2), list_to_set(Ausgabe2, Collection).

% 3 - Es werden alle Stücke mit Komponist bestimmter Komponisten ausgegeben -LARISSA PYCHLAU
collect_st([Kuenstler|KListe], Collection) :- findall((Kuenstler, TITEL), (komponist(KNR, _VORNAME, Kuenstler, _GEBOREN, _GESTORBEN), KNR\==null, stueck(_SNR, KNR, TITEL, _TONART, _OPUS)), Ausgabe3), collect_st(KListe, Ausgabe3_folgend), append(Ausgabe3, Ausgabe3_folgend, Ausgabe_fuer_3_unfertig), list_to_set(Ausgabe_fuer_3_unfertig, Collection).
collect_st([],[]).

% 4 - Gesucht sind Name und Vorname aller Komponisten, die für ein bestimmtes Instrument komponiert haben -ANTON BOSENICK
collect_komp(INSTRUMENT,Collection) :- findall((VORNAME, NAME), (solist(_CDNR, SNR, _NAME, INSTRUMENT), SNR\==null, stueck(SNR, KNR, _TITEL, _TONART, _OPUS), KNR\==null, komponist(KNR, VORNAME, NAME, _GEBOREN, _GESTORBEN), VORNAME\==null, NAME\==null), Ausgabe4), list_to_set(Ausgabe4, Collection).

% 5 - Gesucht ist die Gesamtspieltzeit aufaddiert für individuell jeden Komponisten -ANTON BOSENICK
collect_time(Collection) :- findall((VORNAME, NAME, GESAMTSPIELZEIT, CDNR), (komponist(KNR, VORNAME, NAME, _GEBOREN, _GESTORBEN), KNR\==null, stueck(SNR, KNR, _TITEL, _TONART, _OPUS), SNR\==null, aufnahme(CDNR, SNR, _ORCHESTER, _LEITUNG), CDNR\==null, cd(CDNR, _NAME, _HERSTELLER, _ANZ_CDS, GESAMTSPIELZEIT), GESAMTSPIELZEIT\==null), Ausgabe5), list_to_set(Ausgabe5, Aufgabe5_Zwischenergebnis), aufsummierer(Aufgabe5_Zwischenergebnis, [], Collection).
aufsummierer([(VORNAME, NAME, GESAMTSPIELZEIT, _)|Listenrest], Akkumulator, Ausgabe5_final) :- \+ member((VORNAME, NAME, _GESAMTSPIELZEIT), Akkumulator), !, append(Akkumulator, [(VORNAME, NAME, GESAMTSPIELZEIT)], Befuellte_Liste), aufsummierer(Listenrest, Befuellte_Liste, Ausgabe5_final).
aufsummierer([(VORNAME, NAME, GESAMTSPIELZEIT, _)|Listenrest], Akkumulator, Ausgabe5_final) :- member((VORNAME, NAME, Alter_Wert), Akkumulator), entferner(Akkumulator, (VORNAME, NAME, Alter_Wert), Akkumulator_erneuert), Neuer_Zeitwert is Alter_Wert+GESAMTSPIELZEIT, append(Akkumulator_erneuert,[(VORNAME, NAME, Neuer_Zeitwert)], Befuellt), aufsummierer(Listenrest, Befuellt, Ausgabe5_final).
aufsummierer([], Akkumulator, Akkumulator).
entferner([], _, []).
entferner([H|T], H, T).
entferner([H|T], X, [H|Ausgabe]) :- entferner(T, X, Ausgabe).


% *** AUFGABE 2 ***

% 1 - Gesucht sind alle Vorfahren eines eingegebenen Namens -LARISSA PYCHLAU
vorfahre(X,Y) :- elternteil(X, Y).
vorfahre(X,Y) :- elternteil(Z, Y), vorfahre(X, Z).

% 1 - Gesucht sind alle Nachkommen eines eingegebenen Namens -LARISSA PYCHLAU
nachkomme(X,Y) :- elternteil(Y, X).
nachkomme(X,Y) :- elternteil(Y, Gesucht), nachkomme(X, Gesucht).

% 2 - Geprüft wird, ob Personen miteinander verheiratet sind, unabhängig von ihrer Reihenfolge -ANTON BOSENICK
eheleute(X,Y) :- verheiratet(X, Y), !.
eheleute(X,Y) :- verheiratet(Y, X).

% 3 - Prüft geschlechtsunabhängig ob X und Y Geschwister sind. -LARISSA PYCHLAU
geschwister(X, Y) :- elternteil(Z, X), elternteil(Z, Y), X\==Y.

% 4 - Ist X Bruder von Y -ANTON BOSENICK
bruder(X, Y) :- geschwister(X, Y), maennlich(X).

% 4 - Ist X Schwester von Y -ANTON BOSENICK
schwester(X, Y) :- geschwister(X, Y), weiblich(X).

% 5 - Vorarbeit  -LARISSA PYCHLAU
grosseltern(GROSSELTER, ENKEL_IN) :- elternteil(GROSSELTER, ELTER), elternteil(ELTER, ENKEL_IN).
urgrosseltern(URGROSSELTER, URENKEL_IN) :- elternteil(URGROSSELTER, GROSSELTER), elternteil(GROSSELTER, ELTER), elternteil(ELTER, URENKEL_IN).

% 5 - ist X Opa von Y)  -LARISSA PYCHLAU
opa(OPA, ENKEL_IN) :- grosseltern(OPA, ENKEL_IN), maennlich(OPA).

% 5 - ist X Oma von Y) -LARISSA PYCHLAU
oma(OMA, ENKEL_IN) :- grosseltern(OMA, ENKEL_IN), weiblich(OMA).

% 5 - ist X Uropa von Y -LARISSA PYCHLAU
uropa(UROPA, URENKEL_IN) :- urgrosseltern(UROPA, URENKEL_IN), maennlich(UROPA).

% 5 - ist X Uroma von Y -LARISSA PYCHLAU
uroma(UROMA, URENKEL_IN) :- urgrosseltern(UROMA, URENKEL_IN), weiblich(UROMA).

% 5 - ist X Mutter von Y -LARISSA PYCHLAU
mutter(MUTTER, KIND) :- elternteil(MUTTER, KIND), weiblich(MUTTER).

% 5 - ist X Vater von Y -LARISSA PYCHLAU
vater(VATER, KIND) :- elternteil(VATER, KIND), maennlich(VATER).

% 5 - ist X die Tante von Y -LARISSA PYCHLAU
tante(TANTE, KIND) :- geschwister(TANTE, Y), elternteil(Y, KIND), weiblich(TANTE).

% 5 - ist X die Onkel von Y -LARISSA PYCHLAU
onkel(ONKEL, KIND) :- geschwister(ONKEL, Y), elternteil(Y, KIND), maennlich(ONKEL).

% 6 - Konsistenzprüfung  -ANTON BOSENICK
% 6 - Prüft ob es X gibt die männlich UND weiblich sind  -ANTON BOSENICK
maenUweibl(Ausgabe) :- findall((X), (maennlich(X), weiblich(X)), Ausgabe).

% 6 - Prüft ob bei verheiratet(...) links ein Mann und rechts eine Frau steht  -ANTON BOSENICK
verhKor(Ausgabe) :- findall((X, Y), (verheiratet(X, Y), \+weiblich(Y), \+maennlich(X)), Ausgabe).


% 6 - Prüft ob alle Eltern einem Geschlecht zugeordnet sind  -ANTON BOSENICK
elterVoll(Ausgabe) :- findall((X, Y), (elternteil(X, Y), \+maennlich(X), \+weiblich(X), \+maennlich(Y), \+weiblich(Y)), Ausgabe).

% 6 - Vorarbeit: geschlecht, um alle Personen erfassen zu können  -ANTON BOSENICK
geschlecht(X) :- weiblich(X).
geschlecht(X) :- maennlich(X).

% 6 - Prüft Prüft ob alle Kinder Eltern haben  -ANTON BOSENICK
wurzel(Ausgabe) :- findall(NAME, (geschlecht(NAME), \+elternteil(_Z, NAME)), Ausgabe).
