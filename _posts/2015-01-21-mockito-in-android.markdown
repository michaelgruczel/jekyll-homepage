---
layout: post
title: Mockito in Android
categories : [home, technical]
---

<h2><a href="#">Mockito in Android Tests verwenden</a></h2>
<p class="meta"><span class="date">Januar 21, 2015</span><span class="posted"> Kategorie: Technisches</span></p>
<div class="style1">
<p>
Der Blog Eintrag klingt unsinnig. Jedem ist klar wie man eine
Dependeny einf&uuml;gt und wie man ein Test schreibt,
aber wenn man Mockito innerhalb von Android verwenden will funktioniert das nicht Out-Of-The-Box.
Da mich das ne Stunde gekostet hat, schreib ich mal einfach auf wie das geht.
Ich gehe der Einfachheit halber einfach davon aus dass Android Studio verwendet wird.<br/>
<br/>
<b>Testklasse:</b>
<PRE>
   ...
   public class YourTest extends TestCase {

      @Override
      protected void setUp() throws Exception {
        super.setUp();
      }

      protected void testSomething() throws Exception {
         TextView simpleExampleTestee = Mockito.mock(TextView.class);
         simpleExampleTestee.setText("test");
         Mockito.verify(simpleExampleTestee, Moskito.once()).setText(Matchers.eq("test"));
      }
   }
   ...

</PRE>


<b>Schritt1:</b><br/>
<br/>
Mockito wie gewohnt als dependency in build.gradle einfügen<br/>

<PRE>
    ...
    androidTest 'org.mockito:mockito-core:1.9.5'
    ...
</PRE>
<br/>
Als Ergebnis wird bei der Ausführung etwas in folgender Art passieren:
<br/>
<PRE>
  Caused by: java.lang.VerifyError: org/mockito/cglib/core/ReflectUtils
    at org.mockito.cglib.core.KeyFactory$Generator.generateClass(KeyFactory.java:167)
    at org.mockito.cglib.core.DefaultGeneratorStrategy.generate(DefaultGeneratorStrategy.java:25)
    ...
</PRE>
<br/>
<b>Schritt 2:</b><br/>
<br/>
Mockito dexmaker einfügen, also hat man dann etwas derartiges:<br/>

<PRE>
    ...
    androidTestCompile "org.mockito:mockito-core:1.9.5"
    androidTestCompile "com.google.dexmaker:dexmaker:1.0"
    androidTestCompile "com.google.dexmaker:dexmaker-mockito:1.0"
    ...
</PRE>

Das sollte schon funktionieren auf einem echten Handy,
in der CI (Emulator) kam es dann aber zu folgendem Fehler

<PRE>
    java.lang.IllegalArgumentException: dexcache == null (and no default could be found; consider setting the 'dexmaker.dexcache' system property)
</PRE>
<br/>
<b>Schritt 3:</b><br/>
<br/>
Dexmaker cache setzen, dann sollte es nun auch auf emulatoren laufen, also zum Beispiel in folgender Art<br/>

<PRE>

...
public class YourTest extends TestCase {

  @Override
  protected void setUp() throws Exception {
    super.setUp();
    System.setProperty("dexmaker.dexcache", getContext().getCacheDir().toString());
  }

  protected void testSomething() throws Exception {
    TextView simpleExampleTestee = Mockito.mock(TextView.class);
    simpleExampleTestee.setText("test");
    Mockito.verify(simpleExampleTestee, Moskito.once()).setText(Matchers.eq("test"));
  }
}
...

</PRE>


</p>
</div>

<hr/>
