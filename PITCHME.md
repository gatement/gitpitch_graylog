@title[Title]
## <span class="gold">Graylog</span>
##### Enterprise Log Management 

---

@title[Outline]
#### Outline
<br>
- Introduction |
- Architecture |
- Data Ingesting |

---

@title[Introduction]
##### Graylog is a <span class="gold">centralized logging system</span>
- Storing
- Exploration
- Alerting
- Reporting
<br/>
<span class="aside">and more...</span>

---

@title[Architecture]

- <span class="gold">MongoDB</span>: storing meta information and configuration data
- <span class="gold">Elasticsearch</span>: responsible for log data storing
- <span class="gold">Graylog-Server</span>: the input/output engine and Web UI, on top of MongoDB and Elasticsearch.

+++

@title[Octocat De Los Muertos]
<span style="color:gray; font-size:0.7em">Small setup</span>
<br/>
![Image-Absolute](assets/images/architec_small_setup.png)

+++

@title[Octocat De Los Muertos]
<span style="color:gray; font-size:0.7em">Multi-node setup</span>
<br/>
![Image-Absolute](assets/images/architec_bigger_setup.png)
