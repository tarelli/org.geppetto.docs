***********
Geppetto Meta-Model 
***********

This section explains the Geppetto Model abstraction, its role in Geppetto and how it can be used to provide support for additional domain-specific formats - in other words, being able to extract information from domain-specific content (e.g. native input files for a given simulator) and interact with that information from Geppetto.


The Geppetto Meta-Model is defined in a declarative way using Ecore, a tool for building meta-models from the Eclipse Modeling Framework (EMF). Introductory documentation for EMF can be found at the `eclipse project website <http://www.eclipse.org/modeling/emf/docs/?>`_.



**Main concepts**

Every project that is loaded in Geppetto has a Geppetto model associated to it, the Geppetto Meta-Model is simply a description of what can be found inside a Geppetto model, hence the "Meta".
The main concepts to understand from the Geppetto Meta-Model are Type, Variable and Value, and they will be familiar to pretty much every developer.
A Type represents the structure of an entity therefore defines something that can be associated to one of multiple variables. In modern programming languages this same concept is often referred to as Class.
There are different kind of types defined in the Geppetto meta-model; the CompositeType allows the developer to specify structured types which contain one or more variables inside it.
A variable represents an instance of a given type. This concept exists in every programming language. Geppetto allows a variable to instantiate one or multiple types, a feature that makes it possible to support multi-scale definitions (imagine a variable that at one scale is defined simply as a parameter and at another scale is mapped to a whole computational model with sophisticated dynamics).
A value is something that can be assigned to a variable or to a type (the default value) a concept that once again exists in every programming language. 
There are different kind of values defined in the Geppetto meta-model and every existing type has pretty much a corresponding value defined. 

These are the main concepts to understand, let's have a look at some examples that will show how the model abstraction can be used in practice. In these examples we will use the JAVA API that is generated for us by EMF to create the models.


***Creating a cell***

Let's say we want to create a simple type that represents a biological cell, what would we need to do?

.. code-block:: java
	
	SimpleType cellType = TypesFactory.eINSTANCE.createSimpleType();
	cellType.setId("cell");
	cellType.setName("Cell");
	
	GeppettoLibrary myLibrary = GeppettoFactory.eINSTANCE.createGeppettoLibrary();
	myLibrary.setId("myLibrary");
	myLibrary.getTypes().add(cellType);
	

That's easy right? With these simple three lines we are creating a new simple type in Geppetto. A simple type only has a name, what will be displayed in the UI, and an id which is what will be used every time we want to access the type. We have then added our brand new type to a brand new library we created. 
If we were to feed this model to Geppetto the type with its two fields contained inside "myLibrary" is what we'd see. 
Let's see how difficult it is to see a Sphere in the Geppetto 3D canvas when we instantiate our cell.

First thing we want a handle of a what we call a VisualType. A VisualType tells Geppetto that something can be visualised and comes from inside the Geppetto Common Library. The Geppetto Common Library is a collection of types that Geppetto instantiates by default and that can be used by every domain and application. It is no different from the "myLibrary" you created above, only this one comes from Geppetto.

.. code-block:: java

	VisualType visualType = (VisualType) commonLibrary.getType(TypesPackage.Literals.VISUAL_TYPE);
	
	Variable soma = VariablesFactory.eINSTANCE.createVariable();
	soma.setId("soma");
	soma.getTypes().add(visualType);
	
	Sphere sphere = ValuesFactory.eINSTANCE.createSphere();
	sphere.setRadius(10);
	Point origin = ValuesFactory.eINSTANCE.createPoint(); //x=0,y=0,z=0 by default
	sphere.setPosition(origin);
	
	soma.getInitialValues().put(visualType, sphere);
	
	CompositeVisualType morphology = TypesFactory.eINSTANCE.createCompositeVisualType();
	morphology.setId("morphology");
	morphology.getVariables.add(soma);
	
	cellType.setVisualType(morphology);

So what have we done? We have created a variable we called "soma" and we have made it of type VisualType reusing what comes from the commonLibrary. We have created a Sphere which is one of the VisualValues avaialble through the Geppetto Model, we gave it radius 10 and we placed it at the origin of our scene. We have then assigned this sphere as the initial value of our variable soma.
What we created next is "morphology", a CompositeVisualType to which we have added the soma we just created.
The last line assigns this morpholoy to the cell we created.
What would we see if we were to feed this type to the frontend?

.. image:: images/model/sphere.png
	
That looks good! However you might be wondering, how comes we are seeing anything at all in the screen? We just created a cellType, nobody instantiated anything, you said this was going to be like any other programming language! Which is all correct, you need to instantiate a type if you want it to come into existance and we did it, we haven't just shown you, here it is how you do it:

.. code-block:: java

	Variable myCell = VariablesFactory.eINSTANCE.createVariable();
	myCell.setId("myCell");
	myCell.getTypes().add(cellType)
	
	geppettoModel.getLibraries().add(myLibrary);
	geppettoModel.getVariables().add(myCell);

So this is how you instantiate something, just as you'd expect. We create a variable of the type that we want, cellType in this case and we add it at the root level in the geppettoModel which in this case represents the Java object of our Geppetto Model.
	
	

**Model**

This is the top-level package and contains many of the Geppetto abstractions. 

.. image:: images/model/model.png

GeppettoModel is the EClass that represents the top level node of a Geppetto Model.
Node is an abstract EClass, extended by many entities, which gives the ability to associate an id, a name and a set of Tags to every entity.
The Geppetto Library is simple a container for types. In a Geppetto Model there can be one or multiple libraries defined.

**Types**

This package contains the definition of all the types defined in the Geppetto Meta-model.

.. image:: images/model/types.png
 
An abstract type, simply called Type, is defined and is extended by every existing type.
Every Type can have zero or many superTypes (multiple hierarchy that is), an optional VisualType (which specifies how that type can visualised in the 3D environment) and an optional DomainModel (to specify what domain is declaring that particular type). 

The Geppetto Meta-model defines a set of types to represent dynamic systems. These types can be used by every developer that wish to extend Geppetto to add support for a particular modeling specification.

A Quantity defines the result of a measure. When we associate a Unit to a Quantity we obtain a PhysicalQuantity.
StateVariableType and ParameterType define respectively a state variable and a parameter of a system.
Dynamics describes the dynamics of the system specifying a Function and a PhysicalQuantity as initalCondition.
A Function is defined as an Expression and a list of Arguments. 

An ArrayType defines a type that when instantiated will result 
A VisualType is an abstract EClass that defines a particular kind of type that can be visualised in the 3D environment. A VisualType only allows for a VisualValue to be associated to it (e.g. a Cylinder, a Sphere, an OBJ, etc.).
 

**Values**

This package contains the defintion of all the values that can be associated to variables and types.

.. image:: images/model/values.png

A special mention to CompositeValue that defines a structure value that can be assigned to a variable of type CompositeType.
VisualValues can be assigned to variables of type VisualType.
ArrayValues can be assigned to variables of type ArrayType and specifies the index for each one of the individual values.

**Variable**

This package contains the definition of the variable EClass.

.. image:: images/model/variables.png

**Why EMF?**

The Eclipse Modelling Framework is an industry grade technology which has been around for more than 15 years and is currently used in thousand of professional software and tools.
Ecore allows the developer to specify all the entities (called EClass) and relationships that exist in a given meta-model allowing the developer to define all the constraints (e.g. containment, hierarchy, boundary conditions, etc.) that exist in the model in a declarative way.
EMF adds the ability to generate, from the model definition, the code to use the model in a multitude of languages, making pretty much every line of model-related code bug free.
EMF supports XMI, a dialect of XML, as default serialization standard, making it easy to serialize and deserialize models in a robust way, performing a validation against the schema through every step of the way.   
Geppetto takes advantage also of EMF-JSON an extension that makes it possible to serialize the models also to JSON.
