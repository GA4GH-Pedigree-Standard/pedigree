########
Examples
########

.. toctree::
   :maxdepth: 1


Examples
===========

The following examples demonstrate the way in which pedigrees of various complexity can be represented using the pedigree conceptual model.

Any pedigree more complex than would be represented with a PED file should use the conceptual model implemented within a compatible standard, such as FHIR or Phenopacket.


Basic Trio
------------

A basic family trio consists of one male parent, one female parent, and a proband child. This would be represented as a Pedigree with three Individuals and two parent-child Relationships:

As a Phenopacket `GA4GHPedigree` message:

.. code-block:: yaml

  id: FAM1
  narrative: A Phenopacket GA4GHPedigree of a trio with an affected child
  date: 2022-06-23
  individuals:
    - id: 1
      subject:
        id: MOTHER
        sex: FEMALE
    - id: 2
      subject:
        id: FATHER
        sex: MALE
    - id: 3
      subject:
        id: CHILD
        sex: UNKNOWN
  relationships:
    - individual_id: MOTHER
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: CHILD
    - individual_id: FATHER
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: CHILD
  index_patients:
    - CHILD


Twins
----------

The relationship between twins (TWIN1 and TWIN2) can be represented by adding another Individual, parent-child relationships and a twin Relationship to the Pedigree:

.. code-block:: yaml

  id: FAM2
  narrative: A Phenopacket GA4GHPedigree of a couple with identical twins
  date: 2022-06-23
  individuals:
    - id: 1
      subject:
        id: MOTHER
        sex: FEMALE
    - id: 2
      subject:
        id: FATHER
        sex: MALE
    - id: 3
      subject:
        id: TWIN1
        sex: UNKNOWN
    - id: 4
      subject:
        id: TWIN2
        sex: UNKNOWN
  relationships:
    - individual_id: MOTHER
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: TWIN1
    - individual_id: FATHER
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: TWIN1
    - individual_id: TWIN1
      relation:
        id: KIN:010
        label: isMonozygoticMultipleBirthSiblingOf
      relative_id: TWIN2


The parent-child relationships for TWIN2 are not strictly necessary.
Because the `isMonozygoticTwinOf` relationship is symmetric, it would be equally valid to have said that TWIN2 isMonozygoticTwinOf TWIN1.


Adoption
----------


.. code-block:: yaml


  id: FAM3
  narrative: A Phenopacket GA4GHPedigree of a child with an adoptive mother
  date: 2022-06-23
  individuals:
    - id: 1
      subject:
        id: MOTHER
        sex: FEMALE
    - id: 2
      subject:
        id: BIOLOGICAL_MOTHER
        sex: FEMALE
    - id: 3
      subject:
        id: FATHER
        sex: MALE
    - id: 4
      subject:
        id: CHILD
        sex: UNKNOWN
  relationships:
    - individual_id: MOTHER
      relation:
        id: KIN:022
        label: isAdoptiveParentOf
      relative_id: CHILD
    - individual_id: BIOLOGICAL_MOTHER
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: CHILD
    - individual_id: FATHER
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: CHILD


IVF
-----

.. code-block:: yaml

  id: FAM4
  narrative: A Phenopacket GA4GHPedigree of a child with an egg donor, gestational carrier, and biological father
  date: 2022-06-23
  individuals:
    - id: 1
      subject:
        id: MOTHER
        sex: FEMALE
    - id: 2
      subject:
        id: SURROGATE
        sex: FEMALE
    - id: 3
      subject:
        id: FATHER
        sex: MALE
    - id: 4
      subject:
        id: CHILD
        sex: UNKNOWN
  relationships:
    - individual_id: MOTHER
      relation:
        id: KIN:038
        label: isOvumDonorOf
      relative_id: CHILD
    - individual_id: SURROGATE
      relation:
        id: KIN:005
        label: isGestationalCarrierOf
      relative_id: CHILD
    - individual_id: FATHER
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: CHILD


Complete cancer family
---------------------------

.. figure:: images/classic-brca1-pedigree.jpeg
   :alt: BRCA1 pedigree example

   Example BRCA1 pedigree. Source: https://visualsonline.cancer.gov/details.cfm?imageid=10436


.. code-block:: yaml

  id: FAM5
  narrative: A Phenopacket GA4GHPedigree of a classic BRCA1 pedigree
  date: 2022-06-23
  individuals:
    - id: 1
      subject:
        id: 1
        sex: MALE
        vital_status: DECEASED
    - id: 2
      subject:
        id: 2
        sex: FEMALE
        vital_status: DECEASED
    - id: 3
      subject:
        id: 3
        sex: MALE
        vital_status: DECEASED
    - id: 4
      subject:
        id: 4
        sex: FEMALE
        vital_status: DECEASED
        diseases:
          - term:
              id:
              label: Ovarian cancer
            onset:
              age: P49Y
    - id: 5
      subject:
        id: 5
        sex: FEMALE
    - id: 6
      subject:
        id: 6
        sex: FEMALE
    - id: 7
      subject:
        id: 7
        sex: MALE
    - id: 8
      subject:
        id: 8
        sex: FEMALE
        diseases:
          - term:
              id:
              label: Breast cancer
            onset:
              age: P42Y
    - id: 9
      subject:
        id: 9
        sex: MALE
    - id: 10
      subject:
        id: 10
        sex: FEMALE
    - id: 11
      subject:
        id: 11
        sex: FEMALE
        diseases:
          - term:
              id:
              label: Ovarian cancer
            onset:
              age: P53Y
    - id: 12
      subject:
        id: 12
        sex: FEMALE
    - id: 13
      subject:
        id: 13
        sex: MALE
    - id: 14
      subject:
        id: 14
        sex: FEMALE
    - id: 15
      subject:
        id: 15
        sex: FEMALE
        diseases:
          - term:
              id:
              label: Breast cancer
            onset:
              age: P38Y
  relationships:
    - individual_id: 1
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 5
    - individual_id: 2
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 5
    - individual_id: 1
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 6
    - individual_id: 2
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 6
    - individual_id: 1
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 7
    - individual_id: 2
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 7
    - individual_id: 3
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 8
    - individual_id: 4
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 8
    - individual_id: 3
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 9
    - individual_id: 4
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 9
    - individual_id: 3
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 11
    - individual_id: 4
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 11
    - individual_id: 3
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 12
    - individual_id: 4
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 12
    - individual_id: 7
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 13
    - individual_id: 8
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 13
    - individual_id: 9
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 14
    - individual_id: 10
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 14
    - individual_id: 7
      relation:
        id: KIN:028
        label: isBiologicalFatherOf
      relative_id: 15
    - individual_id: 8
      relation:
        id: KIN:027
        label: isBiologicalMotherOf
      relative_id: 15
  index_patients:
    - 14

