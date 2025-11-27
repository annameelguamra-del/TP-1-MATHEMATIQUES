    #include <stdio.h>

    // -------- PGCD --------
    long long pgcd(long long a, long long b) {
    if (a < 0) a = -a;
    if (b < 0) b = -b;
    while (b != 0) {
        long long r = a % b;
        a = b;
        b = r;
    }
    return a;
    }

    // -------- PPCM --------
    long long ppcm(long long a, long long b) {
    long long d = pgcd(a, b);
    return (a / d) * b;
    }

    // -------- Euclide Étendu --------
    long long euclide_etendu(long long a, long long b, long long *x, long long *y) {
    if (b == 0) {
        *x = (a >= 0) ? 1 : -1;
        *y = 0;
        return (a >= 0) ? a : -a;
    }
    long long x1, y1;
    long long d = euclide_etendu(b, a % b, &x1, &y1);
    *x = y1;
    *y = x1 - (a / b) * y1;
    return d;
    }

    // -------- Fonction 1 : Résolution ax + by = c --------
    void equationLineaire() {
    long long a, b, c;
    printf("\n=== Resolution de l'equation a*x + b*y = c ===\n");

    printf("Entrer a : ");
    scanf("%lld", &a);

    printf("Entrer b : ");
    scanf("%lld", &b);

    printf("Entrer c : ");
    scanf("%lld", &c);

    long long d = pgcd(a, b);
    printf("\nPGCD(%lld, %lld) = %lld\n", a, b, d);
    printf("PPCM(%lld, %lld) = %lld\n", a, b, ppcm(a, b));

    if (c % d != 0) {
        printf("\n=> Pas de solution car %lld ne divise pas %lld.\n", d, c);
        return;
    }

    long long x0, y0;
    euclide_etendu(a, b, &x0, &y0);

    long long k = c / d;
    long long xp = x0 * k;
    long long yp = y0 * k;

    long long pasX = b / d;
    long long pasY = a / d;

    printf("\n=> Solution particuliere : x = %lld, y = %lld\n", xp, yp);
    printf("\nSolution generale :\n");
    printf("x = %lld + %lld*t\n", xp, pasX);
    printf("y = %lld - %lld*t\n", yp, pasY);

    // ---- Calcul de t pour une solution donnée ----
    long long X, Y;
    printf("\nEntrer une solution candidate (x y) : ");
    scanf("%lld %lld", &X, &Y);

    if (pasX != 0) {
        long long t = (X - xp) / pasX;
        if (xp + pasX * t == X && yp - pasY * t == Y)
            printf("=> t = %lld (solution valide)\n", t);
        else
            printf("=> (x,y) ne correspond à aucune valeur de t.\n");
    } 
    else if (pasY != 0) {
        long long t = (yp - Y) / pasY;
        if (xp + pasX * t == X && yp - pasY * t == Y)
            printf("=> t = %lld (solution valide)\n", t);
        else
            printf("=> (x,y) ne correspond à aucune valeur de t.\n");
    } 
    else {
        printf("Cas anormal : a = b = 0.\n");
    }
    }


    // -------- Vérifier si nombre premier --------
    int estPremier(int x) {
    if (x < 2) return 0;
    for (int i = 2; (long long)i * i <= x; i++) {
        if (x % i == 0) return 0;
    }
    return 1;
    }

    // -------- Fonction 2 : Affichage nombres premiers --------
    void afficherPremiers() {
    int n;
    printf("\n=== Affichage des nombres premiers < n ===\n");
    printf("Entrer n : ");
    scanf("%d", &n);

    printf("\nNombres premiers < %d :\n", n);

    for (int i = 2; i < n; i++) {
        if (estPremier(i))
            printf("%d ", i);
    }

    printf("\n");
    }


    int main() {

    int choix;

    while (1) {
        printf("\n=== MENU PRINCIPAL ===\n");
        printf("1 - Resoudre ax + by = c (avec calcul de t)\n");
        printf("2 - Afficher les nombres premiers < n\n");
        printf("Votre choix : ");
        scanf("%d", &choix);

        if (choix == 1)
            equationLineaire();
        else if (choix == 2)
            afficherPremiers();
        else
            printf("Choix invalide.\n");
    }

    return 0;
    }
