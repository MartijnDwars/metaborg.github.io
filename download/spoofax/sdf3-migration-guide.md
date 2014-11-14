## Migration Guide for new SDF3

### Changes in build.main.xml

Add the properties that corresponds to the pp and signature modules (replace <MainModuleName> by the name of your main SDF module) to the **\<!-- Key input modules -->** section:

	<property name="ppmodule" value="<MainModuleName>-pp"/>
	<property name="sigmodule" value="<MainModuleName>-sig"/> 
	
Add the properties that points to the new folders to the **\<!-- Project directories -->** section:	
	
	<property name="sdf-src-gen" location="src-gen"/>
	<property name="pp" location="${sdf-src-gen}/pp"/>
	<property name="signatures" location="${sdf-src-gen}/signatures"/>
	<property name="lib-gen" location="${include}"/>
	<property name="syntax.rel" location="${syntax}" relative="yes"/>
	<property name="trans.rel" location="trans" relative="yes"/>
	<property name="include.rel" location="${include}" relative="yes"/>
	<property name="lib-gen.rel" location="${lib-gen}" relative="yes"/>
	
Also change the **syntax** property to point to the src-gen folder in the same section:
	
	<property name="syntax" location="${sdf-src-gen}/syntax"/>
	
Add the properties necessary to run the Ant Macros to the **\<!-- Environment configuration for command-line builds -->** section:

	<property name="nativepath" value="${eclipse.spoofaximp.nativeprefix}"/>	

	


