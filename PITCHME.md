@title[Title]
## <span class="gold">Graylog</span>
##### Enterprise Log Management 

---

@title[Outline]
#### Outline
- Introduction |
- Architecture |
- Data Ingesting |

---
@title[Introduction1]
#### Application Data Types
![Image-Absolute](assets/images/application_data_types.png)

---

@title[Introduction2]
#### Graylog is a <span class="gold">centralized logging system</span>
- Storing
- Exploration
- Alerting
- Reporting
<br/>
<span class="aside">and more...</span>

---

@title[Architecture]
#### Components
- <span class="gold">MongoDB</span>: storing meta information and configuration data
- <span class="gold">Elasticsearch</span>: storing log data
- <span class="gold">Graylog-Server</span>: input/output/processing/Web UI

+++

@title[Small Setup]
<span style="color:gray; font-size:0.7em">Small setup</span>
<br/>
![Image-Absolute](assets/images/architec_small_setup.png)

+++

@title[Multi-node Setup]
<span style="color:gray; font-size:0.7em">Multi-node setup</span>
<br/>
![Image-Absolute](assets/images/architec_bigger_setup.png)

---

@title[Data Ingesting]
#### Data Ingesting
- Formats: Raw/Plaintext, Syslog, CEF, GELF, ...
- Transport methods: TCP, UDP, Kafka, AMQP, ...

+++

#### [<span class="gold">GELF</span>](http://docs.graylog.org/en/2.4/pages/gelf.html) (<span class="gold">G</span>raylog <span class="gold">E</span>xtended <span class="gold">L</span>og <span class="gold">F</span>ormat) is a log format.
- Structured
- Chunking
- Compression (GZIP/GLIB)

+++

<p><span class="menu-title slide-title">GELF example</span></p>
```javascript
{
  "version": "1.1",
  "host": "example.org",
  "short_message": "A short message that helps you identify what is going on",
  "full_message": "Backtrace here\n\nmore stuff",
  "timestamp": 1385053862.3072,
  "level": 1,
  "_user_id": 9001,
  "_some_info": "foo",
  "_some_env_var": "bar"
}
```javascript

+++

#### <span class="gold">GELF</span> + <span class="gold">UDP</span>
#### <span class="gold">UDP</span> is not reliable, but it is non-blocked.

---

