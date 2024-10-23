---
---

# Defenses and Publications

The following PhD theses have been defended by SeReCo students:
 - **Yousouf Taghzouti**, [Semantic Content Negotiation: a Fine-grained and Relaxed Approach Using Semantic Web Technologies](). Mines Saint-Étienne, 2024. <a style="font-size: small;" href="https://fayol.wp.imt.fr/2024/04/09/felicitations-a-yousouf-taghzouti-pour-sa-soutenance-de-these-sur-la-negociation-de-contenu-semantique-pour-lechange-de-connaissances-entre-systemes-heterogenes/">(fr)</a>
 - **José M. Giménez-García**, [Formalizing, Capturing, and Managing the Context of Statements in the Semantic Web](https://theses.hal.science/tel-04019291v1). Université Jean Monnet, 2022.

Joint work by SeReCo members has lead the following publications (_non-exhaustive list_):

<ul id="publist"></ul>

<script type="text/javascript">
    function query(q) {
        return fetch(
            'https://sparql.dblp.org/sparql',
            {
                method: 'POST',
                body: q,
                headers: {
                    'Content-Type': 'application/sparql-query',
                    'Accept': 'application/sparql-results+json'
                }
            }
        )
        .then(res => res.json());
    }

    const publist = document.getElementById('publist');

    const members = {
        'EMSE': ['91/1410', '151/2525', '40/6463'], // AZ, VC, GN
        'UJM': ['36/3992', '135/4994'], // PM, KS
        'FAU': ['h/AndreasHarth'], // AH
        'KIT': ['121/4546'], // TK
        'USG': ['16/9863', '17/11537'] // SM, AC
    }

    const pid = 'https://dblp.org/pid/';

    let orgs = Object.keys(members);

    let pairs = [];
    for (let i=0; i < orgs.length; i++) {
        for (let j=i+1; j < orgs.length; j++) {
            for (let a1 of members[orgs[i]]) {
                for (let a2 of members[orgs[j]]) {
                    pairs.push(`( <${pid}${a1}> <${pid}${a2}> )`);
                }
            }
        }
    }

    let q = 'prefix dblp: <https://dblp.org/rdf/schema#> '
        + 'prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> '
        + 'prefix xsd: <http://www.w3.org/2001/XMLSchema#> '
        + 'select distinct ?id ?title ?pub ?year where { '
        + `values (?a1 ?a2) { ${pairs.join(' ')} } `
        + '?id dblp:authoredBy ?a1, ?a2 ; dblp:title ?title ; dblp:publishedIn ?pub ; dblp:yearOfPublication ?year . '
        + 'filter (?year >= "2022"^^xsd:gYear)' // start date of SeReCo
        + '} order by desc(?year)';

    // TODO add authors

    query(q)
    .then(a => {
        let l = a.results.bindings.map(mu => {
            id = mu.id.value;
            title = mu.title.value;
            pub = mu.pub.value;
            year = mu.year.value;

            return `<li><a href="${id}">${title}</a> ${pub}, ${year}.</li>`;
        }).join('');

        publist.innerHTML = l;
    })
    .catch(() => {
        publist.innerHTML = '<i>An error occurred while fetching data from <a href="https://dblp.org/">DBLP</a>...</i>';
    });
</script>