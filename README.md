# Feature Envy

## Descripció

Aquest codi pudent forma part de la família dels **Couplers**, i correspon a la categoria d’acoblament excessiu de codi, ja que consisteix en un o diversos **mètodes d’una mateixa classe que criden més dades d’una altra classe que de la pròpia classe** on operen. I el seu nom fa referència precisament a l’”enveja” aparent de les classes que utilitzen en excés mètodes de classes foranes, als quals **sovint no afegeixen cap funcionalitat nova**.

![Feature Envy](https://refactoring.guru/images/refactoring/content/smells/feature-envy-01.png?id=f520a24562e3f4b7848e)


## Problemàtica

Com el compilador no indica errors en la distribució de les classes, aquest codi pudent pot derivar en **bugs addicionals** quan se n’arreglen d’existents, per la falta de centralització dels mètodes, i **fa més dificultós afegir característiques a l’aplicaicó** ja que s’han de modificar més classes de les necessàries al mateix temps.

```
public class Featureenvy1 {
    private Featureenvy2 telefonMobil;
    
    //Aquest mètode genera el codi pudent, ja que crida de forma exclusiva dades d'una altra classe a la qual no pertany.
    public String getFormatTelefon() {
        return "("
                + telefonMobil.getCodiArea() + ") "
                + telefonMobil.getPrefix() + "-"
                + telefonMobil.getNumero();
    }
}
public class Featureenvy2 {
    private final String numeroSenseFormat;
    public Featureenvy2(String numeroSenseFormat) {
        this.numeroSenseFormat = numeroSenseFormat;
    }
    public String getCodiArea() {
        return numeroSenseFormat.substring(0, 3);
    }
    public String getPrefix() {
        return numeroSenseFormat.substring(3, 6);
    }
    public String getNumero() {
        return numeroSenseFormat.substring(6, 10);
    }
}
```

## Solució

La situació ideal seria que si les dades d’un objecte canvien, fossin majoria en el mètode que les modifica, i aquest mètode es trobés el més pròxim possible, és a dir dins de la mateixa classe. Per tant, això s’assoleix *canviant la ubicació del mètode a la classe que crida més sovint* **(Move method)**, o bé *fragmentant el mètode i desplaçant aquella part que processa una majoria de dades d’una classe a aquest mateix fitxer* **(Extract method)**.


```
public class Featureenvy1 {
    //Classe amb el problema resolt que crida un únic mètode de l'altra classe, en comptes de cridar 3 funcions diferents
    d'aquesta, per tal de centralitzar el codi corresponent a la classe Featureenvy2.
    private Featureenvy2 telefonMobil;
    public String telefonFinal() {
        
        return telefonMobil.formatejarTelefon();
        
    }
}
public class Featureenvy2 {
    private final String numeroSenseFormat;
    public Featureenvy2(String numeroSenseFormat) {
        this.numeroSenseFormat = numeroSenseFormat;
    }
    public String getCodiArea() {
        return numeroSenseFormat.substring(0, 3);
    }
    public String getPrefix() {
        return numeroSenseFormat.substring(3, 6);
    }
    public String getNumero() {
        return numeroSenseFormat.substring(6, 10);
    }
    
    //S'ha exportat aquest mètode des de la classe Featureenvy1 per ubicar-lo a la classe d'on extreu tota la informació
    i concentrar les operacions sobre el numero de telèfon en aquesta.
    public String formatejarTelefon() {
        return "("
                + getCodiArea() + ") "
                + getPrefix() + "-"
                + getNumero();
    }
}
```

## Avantatges

L’avantatge principal és que en centralitzar els mètodes en les seves classes corresponents s’eviten reproduccions innecessàries en altres classes i **prevé la duplicació del codi**, a més de l’aspecte organitzatiu a l’hora de localitzar els mètodes, que romandran a prop de la majoria de dades que utilitzen.

## Inconvenients

Aquest tipus de codi pudent no ha de representar necessàriament un problema, com en el cas que el vulgui separar intencionadament les dades de les operacions que s’en fan, a més del fet que centralitzar les operacions sobre les dades en una mateixa classe **pot derivar en un altre codi pudent de tipus Large Class** per les grans proporcions del codi resultant.

## Webgrafia

[Referència 1](https://refactoring.guru/es/smells/feature-envy) / 
[Referència 2](https://elearning.industriallogic.com/gh/submit?Action=PageAction&album=recognizingSmells&path=recognizingSmells/featureEnvy/featureEnvyExample&devLanguage=Java) / 
[Referència 3](https://waog.wordpress.com/2014/08/25/code-smell-feature-envy/) / 
[Referència 4](https://dev.to/thinkster/code-smell-feature-envy-3h0p)

## Autor

**Ricardo García** - [Perfil](https://github.com/riga037)
