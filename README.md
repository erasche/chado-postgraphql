## Get Organisms

```graphql
query{
  allOrganisms {
    edges {
      node {
        __id
        organismId
        abbreviation
        genus
        species
        commonName
        infraspecificName
        typeId
        comment
      }
    }
  }
}
```
## Creating an Organism

mutation

```
mutation CreateOrganism($input: CreateOrganismInput!) {
	createOrganism(input: $input) {
		clientMutationId
		organism {
			organismId
		}
	}
}
```

data

```json
{
	"input": {
		"clientMutationId": "549b5e7c-0516-4fc9-8944-125401211590",
		"organism": {
			"abbreviation": "abcd",
			"genus": "Abb",
			"species": "cddd",
			"commonName": "asdf",
			"typeId": null,
			"comment": ""
		}
	}
}
```
