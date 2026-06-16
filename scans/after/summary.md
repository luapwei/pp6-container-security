### Gesamtergebnis Vorher → Nachher

|Metrik|Vorher|Nachher|Delta|
|---|---|---|---|
|CIS Score (Section 4+5)|54,1 %|97,4 %|+43,3 pp|
|Trivy CVEs total|143|0|−143|
|Trivy CRITICAL|2|0|−2|
|Trivy HIGH|47|0|−47|
|Trivy MEDIUM|62|0|−62|
|Trivy LOW|32|0|−32|

### CIS Score-Rechnung (konsistente Logik beide Messungen)

||Vorher|Nachher|
|---|---|---|
|PASS auto|18|30|
|PASS manuell|2|8|
|**Σ PASS**|**20**|**38**|
|WARN auto|11 (13 − 2 OOS)|1 (5.9)|
|FAIL manuell|4 (5 − 1 OOS)|0|
|PARTIAL|2|0|
|**Σ nicht-PASS**|**17**|**1**|
|INFO ausgeschlossen|4|2|
|Out-of-scope|3|3|
|**Scope**|**37**|**39**|
|**Score**|**54,1 %**|**97,4 %**|

**Δ = +43,3 Prozentpunkte.**