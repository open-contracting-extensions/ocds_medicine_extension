# Medicine extension

Adds fields to the item object relevant to the procurement of medicines.

## Guidance

This extension is intended to be used in the medicines-related items in the tender, award, or contract stages, to add more specific details that a medicine item may have. To use it, set the properties that are known, including the active ingredients, dosage form, the medicine container, and the administration route.

If a contracting process is in the award or contract stage, it’s possible to know more information about the medicine, such as the brand, the manufacturer, the country of origin, the expiration date, if they must maintain a cold chain and all the other commercial, financial and logistical conditions. Use the [generic item attributes](https://extensions.open-contracting.org/en/extensions/itemAttributes/master/) extension for all the cases where the medicine item has other attributes not included in this extension.

If a medicine item has more than one active ingredient, add each one to the `activeIngredients` array.

If a medicine item is packaged in a multi-drug container, use `items.quantity` for the quantity in the container and `items.unit` for the unit.

## Examples

### One active ingredient

This is an [example](https://api.mercadopublico.cl/APISOCDS/ocds/tender/734-82-LP14) of an item of a drug procurement process in Chile, and its modeling in the extension.

Description | Minimum dispensing unit
--|--
Acetilcisteina | ACETILCISTEINA-N 100 MG/ML SOLUCION PARA NEBULIZAR FRASCO 15-30 ML ENVASE INDIVIDUAL RESISTENTE CON SELLO QUE ASEGURE INVIOLABILIDAD DEL CONTENIDO

The strength is expressed as "100 MG/ML". The UN/CEFACT [Recommendation 20 – Codes for Units of Measure Used in International Trade](https://unece.org/trade/uncefact/cl-recommendations) codelist includes units like mg/l, g/l and kg/l, but not mg/ml. So, "100 MG/ML" is expressed as 100 g/l below.

```json
{
  "tender": {
    "items": [
      {
        "id": "1",
        "description": "Acetilcisteina",
        "classification": {
          "id": "51161701",
          "scheme": "UNSPSC",
          "uri": "https://apis.mercadopublico.cl/OCDS/data/productos/categoria/51161701"
        },
        "dosageForm": "SOL",
        "administrationRoute": "NASALINSTIL",
        "container": {
          "name": "jar",
          "capacity": {
            "unit": {
              "scheme": "UNCEFACT",
              "id": "ml"
            },
            "value": "[15,30]"
          }
        },
        "activeIngredients": [
          {
            "name": "acetilcisteina",
            "strength": {
              "unit": {
                "scheme": "UNCEFACT",
                "id": "g/l"
              },
              "value": 100
            }
          }
        ]
      }
    ]
  }
}
```
### More than one active ingredient

This is an [example](https://www.contrataciones.gov.py/licitaciones/convocatoria/391507-adquisicion-medicamentos-hospital-clinicas-1.html#pliego) of an item of a drug procurement process in Paraguay and how it would be represented with the extension.

Description | Technical specifications | Unit of measurement | Presentation |  Delivery presentation
--|--|--|--|--
Clorhidrato de Bupivacaina Hiperbarica Inyectable     | clorhidrato de bupivacaina 25 mg. + dextrosa 82,5 mg. - solución inyectable | UNIDAD | VIAL | caja conteniendo 25 ampollas como minimo de 5 ml.

```json
{
  "tender": {
    "items": [
      {
        "id": "1",
        "dosageForm": "SOL",
        "administrationRoute": "TRNSDERM",
        "container": {
          "name": "blstrpk",
          "capacity": {
            "unit": {
              "scheme": "UNCEFACT",
              "id": "ml"
            },
            "value": "[5,INF["
          }
        },
        "activeIngredients": [
          {
            "name": "clorhidrato de bupivacaina",
            "strength": {
              "unit": {
                "scheme": "UNCEFACT",
                "id": "mg"
              },
              "value": 250
            }
          },
          {
            "name": "dextrosa",
            "strength": {
              "unit": {
                "scheme": "UNCEFACT",
                "id": "mg"
              },
              "value": 82.5
            }
          }
        ],
        "quantity": 25,
        "unit": {
          "id": "UNI",
          "name": "Unidad"
        }
      }
    ]
  }
}
```

## Related standards

The fields, definitions and codelists used in this extension are based on the following standards that are commonly used in the data on public medicine purchases.

- Most of the fields are based on the [Drug](https://schema.org/Drug) definition by the [Schema.org Community Group](https://www.w3.org/community/schemaorg/) and the [Medication Resource](https://www.hl7.org/fhir/medication.html) from [Fast Healthcare Interoperability Resources (FHIR)](http://hl7.org/fhir/) standard.
- The `administrationRoute` codelist contains the top-level concepts in HL7's [Route of Administration](https://terminology.hl7.org/CodeSystem/v3-RouteOfAdministration/) codelist, excluding any synonymous terms.
- The `dosageForm` codelist contains the top-level concepts in HL7's [Orderable Drug Form](https://terminology.hl7.org/CodeSystem/v3-orderableDrugForm/) codelist, excluding the specific forms of sprays.
- The `container` codelist is a copy of the codes and titles from FHIR's [Medication Knowledge Package Type](https://terminology.hl7.org/CodeSystem/medicationknowledge-package-type/) codelist. Given that the terms are undefined in FHIR, the descriptions are copied from corresponding terms from the [EDQM Standard Terms database](https://standardterms.edqm.eu), reproduced with the permission of the European Directorate for the Quality of Medicines & HealthCare, Council of Europe (EDQM). The EDQM Standard Terms database is not a static list and content can change over time; the descriptions were retrieved on July 21, 2021.

If you haven't adopeted or can not map your `administrationRoute`, `dosageForm` or `container` values to HL7, you can use your own codes. To make sure the data user undertand them, you should name the codelists used in your publication policy document, and provide access to tables where the codes can be looked up.

## Background

This extension is based on research with 4 data users and 6 data publishers including public entities, journalists, medicine price analysts, and software developers for medicine purchase systems from 9 countries in Latin America, Europe, and Africa. The extension includes the most used fields from the different countries.

## Issues

Report issues for this extension in the [ocds-extensions repository](https://github.com/open-contracting/ocds-extensions/issues), putting the extension's name in the issue's title.
