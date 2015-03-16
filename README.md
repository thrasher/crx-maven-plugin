This plugin makes it easy to build [http://code.google.com/chrome/apps/ Chrome Apps] for the [https://chrome.google.com/webstore Webstore] from Maven projects.  It's especially nice for developing GWT-based hosted apps, but works equally well for hand-crafted HTML and Java Script apps.  Since maven is command-line based, it allows building Chrome apps from continuous integration and build systems, using the CRX file as a natural Maven artifact.

=== News ===

Version 1.1.0 is stable, and has been [http://crx-maven-plugin.googlecode.com/files/crx-maven-plugin-1.1.0.jar released] with [http://code.google.com/p/crx-maven-plugin/wiki/ReleaseNotes notes].

=== About ===

The plugin can be configured in a pom.xml to wrap up any directory as a .crx file, but is designed to work with the maven-war-plugin to stage the target file set normally built from src/main/webapp into a war.

There are other solutions than using this crx-maven-plugin:
 * For Python: [http://code.google.com/p/crx-packaging/ crx-packaging] based on [http://grack.com/blog/2009/11/09/packing-chrome-extensions-in-python/ Packing Chrome extensions in Python]
 * For Ruby: [http://github.com/Constellation/crxmake crxmake]


=== Example Use ===
{{{

<!-- project artifact is a crx -->
<packaging>crx</packaging>
...

<build>
  <plugins>

    <!-- use the war plugin to stage the crx files -->
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-war-plugin</artifactId>
      <executions>
        <execution>
          <id>stage-crx</id>
          <phase>prepare-package</phase>
          <goals>
            <goal>exploded</goal>
          </goals>
        </execution>
      </executions>
    </plugin>

    <!-- the crx is created in maven's package phase -->
    <plugin>
      <groupId>com.google.code</groupId>
      <artifactId>crx-maven-plugin</artifactId>
      <version>1.1.0</version>
      <extensions>true</extensions>
      <configuration>
        <pemKey>mykey.pem</pemKey>
        <pemCert>mycert.pem</pemCert>
      </configuration>
    </plugin>

  </plugins>
</build>
...
}}}

=== Building ===

Build the crx-maven-plugin for use locally by checking it out, and installing it.

{{{
svn checkout http://crx-maven-plugin.googlecode.com/svn/trunk/ crx-maven-plugin
cd crx-maven-plugin
mvn install
}}}

=== Example Apps ===

Three Chrome Apps are built as integration tests in SVN.  These could be a starting point for your own app:
  # maps_app, a [http://code.google.com/chrome/apps/docs/developers_guide.html#live Google Maps demo app]
  # html5rocks, as an app
  # gwt_greet, an offline GWT AJAX app, [http://code.google.com/chrome/apps/docs/developers_guide.html#serverless serverless]

To build them, you must enable the integration test profile:

{{{
# build integration tests
mvn install -P run-its
}}}

Keep in mind that the integration tests run their build with a clean environment. Meaning: they download a clean maven repository in the /target/ for all dependencies needed to build the sample apps.  This takes time (maybe ~30 minutes over a DSL line), so go brew some coffee while waiting.


