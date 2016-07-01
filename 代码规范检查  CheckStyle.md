#CheckStyle

CheckStyle 主要用来检测代码是否规范.下面说一下怎么在 android studio 中使用 checkStyle.

在 project 的 build.gradle 中增加以下配置:


```
allprojects {
    
    ...
    
    apply plugin: 'checkstyle'
    task checkstyle(type: Checkstyle) {
        source 'src'
        include '**/*.java'
        exclude '**/gen/**'
        exclude '**/R.java'
        exclude '**/BuildConfig.java'
        exclude 'test/'
        configFile new File(rootDir, "checkstyle.xml")
        // empty classpath
        classpath = files()
    }
}

```

在 porject 根目录下新建 checkstyle.xml 文件.
```
<?xml version="1.0"?>
<!DOCTYPE module PUBLIC
    "-//Puppy Crawl//DTD Check Configuration 1.2//EN"
    "http://www.puppycrawl.com/dtds/configuration_1_2.dtd">

<module name="Checker">
    <!--module name="NewlineAtEndOfFile"/-->
    <module name="FileLength"/>
    <module name="FileTabCharacter"/>

    <!-- Trailing spaces -->
    <module name="RegexpSingleline">
        <property name="format" value="\s+$"/>
        <property name="message" value="Line has trailing spaces."/>
    </module>

    <!-- Space after 'for' and 'if' -->
    <module name="RegexpSingleline">
        <property name="format" value="^\s*(for|if)[^ ]"/>
        <property name="message" value="Space needed before opening parenthesis."/>
    </module>

    <!-- For each spacing -->
    <module name="RegexpSingleline">
        <property name="format" value="^\s*for \(.*?([^ ]:|:[^ ])"/>
        <property name="message" value="Space needed around ':' character."/>
    </module>

    <module name="TreeWalker">
        <!--<property name="cacheFile" value="${checkstyle.cache.file}"/>-->

        <!-- Checks for Javadoc comments.                     -->
        <!-- See http://checkstyle.sf.net/config_javadoc.html -->
        <!--module name="JavadocMethod"/-->
        <!--module name="JavadocType"/-->
        <!--module name="JavadocVariable"/-->
        <!--module name="JavadocStyle"/-->


        <!-- Checks for Naming Conventions.                  -->
        <!-- See http://checkstyle.sf.net/config_naming.html -->
        <module name="ConstantName"/>
        <module name="LocalFinalVariableName"/>
        <module name="LocalVariableName"/>
        <module name="MemberName"/>
        <module name="MethodName">
            <property name="format" value="^[a-z][a-zA-Z0-9_]*$"/>
        </module>
        <module name="PackageName"/>
        <module name="ParameterName"/>
        <module name="StaticVariableName"/>
        <module name="TypeName">
            <property name="format" value="^[A-Z][a-zA-Z0-9_]*$"/>
        </module>


        <!-- Checks for imports                              -->
        <!-- See http://checkstyle.sf.net/config_import.html -->
        <module name="AvoidStarImport"/>
        <module name="IllegalImport"/>
        <module name="RedundantImport"/>
        <module name="UnusedImports">
            <property name="processJavadoc" value="true"/>
        </module>


        <!-- Checks for Size Violations.                    -->
        <!-- See http://checkstyle.sf.net/config_sizes.html -->
        <module name="LineLength">
            <property name="max" value="100"/>
        </module>
        <!--<module name="MethodLength"/>-->
        <module name="ParameterNumber"/>


        <!-- Checks for whitespace                               -->
        <!-- See http://checkstyle.sf.net/config_whitespace.html -->
        <module name="GenericWhitespace"/>
        <module name="EmptyForIteratorPad"/>
        <module name="MethodParamPad"/>
        <module name="NoWhitespaceAfter"/>
        <module name="NoWhitespaceBefore"/>
        <module name="OperatorWrap"/>
        <module name="ParenPad"/>
        <module name="TypecastParenPad"/>
        <module name="WhitespaceAfter"/>
        <module name="WhitespaceAround"/>


        <!-- Modifier Checks                                    -->
        <!-- See http://checkstyle.sf.net/config_modifiers.html -->
        <!--module name="ModifierOrder"/-->
        <module name="RedundantModifier"/>


        <!-- Checks for blocks. You know, those {}'s         -->
        <!-- See http://checkstyle.sf.net/config_blocks.html -->
        <module name="AvoidNestedBlocks"/>
        <module name="EmptyBlock"/>
        <module name="LeftCurly"/>
        <module name="NeedBraces">
            <property name="tokens" value="LITERAL_DO, LITERAL_ELSE, LITERAL_FOR, LITERAL_WHILE"/>
        </module>
        <module name="RightCurly"/>


        <!-- Checks for common coding problems               -->
        <!-- See http://checkstyle.sf.net/config_coding.html -->
        <!--<module name="AvoidInlineConditionals"/>-->
        <module name="CovariantEquals"/>
        <!--<module name="DoubleCheckedLocking"/>-->
        <module name="EmptyStatement"/>
        <module name="EqualsAvoidNull"/>
        <module name="EqualsHashCode"/>
        <!--<module name="HiddenField"/>-->
        <module name="IllegalInstantiation"/>
        <module name="InnerAssignment"/>
        <!--<module name="MagicNumber"/>-->
        <module name="MissingSwitchDefault"/>
        <module name="RedundantThrows"/>
        <module name="SimplifyBooleanExpression"/>
        <module name="SimplifyBooleanReturn"/>

        <!-- Checks for class design                         -->
        <!-- See http://checkstyle.sf.net/config_design.html -->
        <!--module name="DesignForExtension"/-->
        <!--module name="FinalClass"/-->
        <module name="HideUtilityClassConstructor"/>
        <module name="InterfaceIsType"/>
        <!--<module name="VisibilityModifier"/>-->


        <!-- Miscellaneous other checks.                   -->
        <!-- See http://checkstyle.sf.net/config_misc.html -->
        <module name="ArrayTypeStyle"/>
        <!--module name="FinalParameters"/-->
        <module name="TodoComment"/>
        <module name="UpperEll"/>
    </module>
</module>
```

在 android studio 的Terminal 中执行:
> ./gradlew checkstyle

生成结果在 Moudel 的 build/reports/checkStyle 中. 用浏览器打开checkstyle.html文件即可
