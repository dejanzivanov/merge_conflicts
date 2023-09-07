      <template>
          <div class="container-fluid px-4">
              <h1 class="mt-4">Administracija izvora</h1>
              <ol class="breadcrumb mb-0">
                  <li class="breadcrumb-item"><a href="/">Početna</a></li>
                deki
                  <li class="breadcrumb-item"><a href="/administracija-grupa">Grupe</a></li>
                  <li class="breadcrumb-item active">Izvori</li>
                mile
              </ol>

              <!-- Modal novi izvor -->
              <div class="modal fade" id="createIzvor" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                  <div class="modal-dialog modal-lg">
                      <div class="modal-content">
                          <div class="modal-header">
                              <h5 class="modal-title" id="exampleModalLabel">Kreirajte novi izvor</h5>
                              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                          </div>
                          <div class="modal-body m-2">
                              <form method="POST">
                                  <div class="mb-3">
                                      <label class="form-label required">Naziv izvora:</label>
                                      <input class="form-control" v-model="nazivIzvora" type="text" name="nazivIzvora" placeholder="Unesite naziv izvora" :class="{ 'is-invalid': errors.nazivIzvora }" />
                                      <span v-if="errors.nazivIzvora" class="invalid-feedback">{{ nazivPoruka }}</span>
                                  </div>
                                  <div class="mb-3">
                                      <label class="form-label required">Opis Izvora:</label>
                                      <input class="form-control" v-model="opisIzvora" type="text" name="opisIzvora" placeholder="Unesite opis izvora" :class="{ 'is-invalid': errors.opisIzvora }" />
                                      <span v-if="errors.opisIzvora" class="invalid-feedback">{{ opisPoruka }}</span>
                                  </div>
                                  <br />
                                  <div class="mb-3">
                                      <label class="form-label required">Način Ulaska:</label><br /><br />
                                      <input class="form-check-input" type="radio" name="nacinUlaska" value="rucno" id="rucno" v-model="selectedNacinUlaska" />
                                      <label class="form-check-label" for="rucno"> Ručni unos </label>&emsp;&emsp;

                                      <input class="form-check-input" type="radio" name="nacinUlaska" value="automatski" id="automatski" v-model="selectedNacinUlaska" />
                                      <label class="form-check-label" for="automatski"> Automatski unos </label>
                                  </div>
                                  <br />
                                  <!-- Dodatak za ručni unos -->
                                  <div v-if="selectedNacinUlaska === 'rucno'">
                                      <h5>Lista obrada za ručni unos:</h5>

                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="obradaPoTLD" v-model="obradaTLD" />
                                              <label class="form-check-label" for="obradaPoTLD">Obrada po TLD</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="odbaceniDomeni" v-model="odbaceniDomeni" />
                                              <label class="form-check-label" for="odbaceniDomeni">Obrada po odbačenim domenima</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="kljucneReci" v-model="kljucneReci" />
                                              <label class="form-check-label" for="kljucneReci">Obrada po ključnim recima</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="odbaceneEmail" v-model="odbaceneEmail" />
                                              <label class="form-check-label" for="odbaceneEmail">Odbačene email adrese</label>
                                          </div>
                                      </div>
                                  </div>

                                  <!-- Dodatak za automatski unos -->
                                  <div v-if="selectedNacinUlaska === 'automatski'">
                                      <h5>Postavke automatskog unosa:</h5>
                                      <div class="mb-3">
                                          <label class="form-label required">API ruta:</label>
                                          <input class="form-control" v-model="apiRuta" type="text" name="apiRuta" placeholder="Unesite API rutu npr.http://company.com/company" :class="{ 'is-invalid': errors.apiRuta }" />
                                          <span v-if="errors.apiRuta" class="invalid-feedback">{{ apiPoruka }}</span>
                                      </div>
                                      <div class="mb-3">
                                          <label class="form-label required">Interval automatskog unosa (u minutima):</label>
                                          <input class="form-control" v-model="intervalUnosa" type="text" name="intervalUnosa" min="1" :class="{ 'is-invalid': errors.intervalUnosa }" v-numericOnly />
                                          <span v-if="errors.intervalUnosa" class="invalid-feedback">{{ poruka }}</span>
                                      </div>
                                      <div class="mb-3">
                                          <label class="form-label">Datum do kada se vrši automatski unos:</label>
                                          <date-picker v-model="datumUnosa" format="DD.MM.YYYY."></date-picker><br />
                                          <small class="form-text text-muted">Napomena: Ako je polje prazno, automatski unos nema kraj.</small>
                                      </div>
                                      <h5>Lista obrada za automatski unos:</h5>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="obradaPoTLD" v-model="obradaTLD" />
                                              <label class="form-check-label" for="obradaPoTLD">Obrada po TLD</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="odbaceniDomeni" v-model="odbaceniDomeni" />
                                              <label class="form-check-label" for="odbaceniDomeni">Obrada po odbačenim domenima</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="kljucneReci" v-model="kljucneReci" />
                                              <label class="form-check-label" for="kljucneReci">Obrada po ključnim recima</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="odbaceneEmail" v-model="odbaceneEmail" />
                                              <label class="form-check-label" for="odbaceneEmail">Odbačene email adrese</label>
                                          </div>
                                      </div>
                                  </div>
                              </form>
                          </div>
                          <div class="modal-footer">
                              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Odustani</button>
                              <button @click="dodajIzvor" id="dodajIzvor" type="button" class="btn btn-primary">Dodaj novi izvor</button>
                          </div>
                      </div>
                  </div>
              </div>

              <div class="row mt-3 mb-3">
                  <div class="col-6">
                      <button class="btn btn-outline-primary" type="button" data-bs-toggle="collapse" data-bs-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
                          <i class="fa-solid fa-filter"></i> &nbsp; Filteri
                      </button>
                  </div>
                  <div class="col-6">
                      <button @click="errorsClean" type="button" class="btn btn-primary col-4 float-end" data-bs-toggle="modal" data-bs-target="#createIzvor">Kreirajte Izvor</button>
                  </div>
              </div>

              <!--FIlteri za datetable-->
              <div class="collapse mb-3" id="collapseExample">
                  <div class="card card-body">
                      <div class="row">
                          <div class="col-3">
                              <label class="form-label"><b>Datum Poslednje Obrade</b></label
                              ><br />
                              <date-picker v-model="datumObradeFilter" format="DD.MM.YYYY." @change="izvorDataTable"></date-picker>
                          </div>
                          <div class="col-3">
                              <label class="form-label"><b>Datum Ubacivanja</b></label
                              ><br />
                              <date-picker v-model="datumUbacivanjaFilter" format="DD.MM.YYYY." @change="izvorDataTable"></date-picker>
                          </div>
                          <div class="col-3">
                              <label class="form-label"><b>Status Izvora:</b></label>
                              <select class="form-select" aria-label="Default select example" v-model="statusIzvoraFilter" @change="izvorDataTable">
                                  <option value="">---- Filtrirajte status izvora ----</option>
                                  <option value="Aktivan">Aktivan</option>
                                  <option value="Neaktivan">Neaktivan</option>
                              </select>
                          </div>
                          <div class="col-3">
                              <label class="form-label"><b>Način Ulaska:</b></label>
                              <select class="form-select" aria-label="Default select example" v-model="nacinUlaskaFilter" @change="izvorDataTable">
                                  <option value="">---- Filtrirajte način ulaska ----</option>
                                  <option value="Ručno">Ručno</option>
                                  <option value="Automatski">Automatski</option>
                              </select>
                          </div>
                      </div>
                      <div class="row">
                          <div class="my-3 d-grid gap-2 d-flex justify-content-end">
                              <button @click="clearFilter" type="button" class="btn btn-primary col-2 float-end">Resetujte Filtere</button>
                          </div>
                      </div>
                  </div>
              </div>

              <div class="card mb-4">
                  <div class="card-header">
                      <i class="fas fa-table me-1"></i>
                      Izvori
                  </div>
                  <div class="card-body">
                      <table class="display responsive table table-bordered table-striped pt-2" width="100%" id="izvorTabela">
                          <thead>
                              <tr>
                                  <th>ID</th>
                                  <th>Naziv Izvora</th>
                                  <th>Opis Izvora</th>
                                  <th>Status Izvora</th>
                                  <th>Broj E-Mail Adresa</th>
                                  <th>Način Ulaska</th>
                                  <th>Datum Poslednje Obrade</th>
                                  <th>Dat. Posl. Ubacivanja U Sadržaj</th>
                                  <th>Akcije</th>
                              </tr>
                          </thead>
                          <tbody></tbody>
                      </table>
                  </div>
              </div>

              <!-- Modal za izmenu izvora -->
              <div class="modal fade" id="izmeniIzvor" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                  <div class="modal-dialog modal-lg">
                      <div class="modal-content">
                          <div class="modal-header">
                              <h5 class="modal-title" id="exampleModalLabel">Izmena izvora</h5>
                              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                          </div>
                          <div class="modal-body m-2">
                              <form method="POST">
                                  <!-- Select statusa izvora-->
                                  <div class="mb-3">
                                      <label class="form-label required">Status Izvora:</label>
                                      <select class="form-select" aria-label="Default select example" v-model="statusIzvora">
                                          <option selected>---- Odaberite status izvora ----</option>
                                          <option value="Aktivan">Aktivan</option>
                                          <option value="Neaktivan">Neaktivan</option>
                                      </select>
                                  </div>
                                  <!-- Naziv izvora-->
                                  <div class="mb-3">
                                      <label class="form-label required">Naziv izvora:</label>
                                      <input class="form-control" v-model="nazivIzmeni" type="text" name="nazivIzmeni" :class="{ 'is-invalid': errors.nazivIzmeni }" />
                                      <span v-if="errors.nazivIzmeni" class="invalid-feedback">{{ nazivIzmeniPoruka }}</span>
                                  </div>
                                  <!-- Radio nacin ulaska-->
                                  <div class="mb-3">
                                      <label class="form-label required">Način Ulaska:</label><br /><br />
                                      <input class="form-check-input" type="radio" name="nacinUlaskaIzmena" value="rucno" id="rucnoIzmena" v-model="selectedNacinUlaskaIzmena" />
                                      <label class="form-check-label" for="rucnoIzmena"> Ručni unos </label>&emsp;&emsp;

                                      <input class="form-check-input" type="radio" name="nacinUlaskaIzmena" value="automatski" id="automatskiIzmena" v-model="selectedNacinUlaskaIzmena" />
                                      <label class="form-check-label" for="automatskiIzmena"> Automatski unos </label>
                                  </div>
                                  <br />

                                  <!-- Izmena za ručni unos -->
                                  <div v-if="selectedNacinUlaskaIzmena === 'rucno'">
                                      <h5>Lista obrada za ručni unos:</h5>

                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="obradaTLDIzmena" v-model="obradaTLDIzmena" />
                                              <label class="form-check-label" for="obradaTLDIzmena">Obrada po TLD</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="odbaceniDomeniIzmena" v-model="odbaceniDomeniIzmena" />
                                              <label class="form-check-label" for="odbaceniDomeniIzmena">Obrada po odbačenim domenima</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="kljucneReciIzmena" v-model="kljucneReciIzmena" />
                                              <label class="form-check-label" for="kljucneReciIzmena">Obrada po ključnim recima</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="odbaceneEmailIzmena" v-model="odbaceneEmailIzmena" />
                                              <label class="form-check-label" for="odbaceneEmailIzmena">Odbačene email adrese</label>
                                          </div>
                                      </div>
                                  </div>

                                  <!-- Izmena za automatski unos -->
                                  <div v-if="selectedNacinUlaskaIzmena === 'automatski'">
                                      <h5>Postavke automatskog unosa:</h5>
                                      <div class="mb-3">
                                          <label class="form-label required">API ruta:</label>
                                          <input
                                              class="form-control"
                                              v-model="apiRutaIzmena"
                                              type="text"
                                              name="apiRutaIzmena"
                                              placeholder="Unesite API rutu npr.http://company.com/company"
                                              :class="{ 'is-invalid': errors.apiRutaIzmena }"
                                          />
                                          <span v-if="errors.apiRutaIzmena" class="invalid-feedback">{{ apiPorukaIzmena }}</span>
                                      </div>
                                      <div class="mb-3">
                                          <label class="form-label required">Interval automatskog unosa (u minutima):</label>
                                          <input class="form-control" v-model="intervalUnosaIzmena" type="text" name="intervalUnosaIzmena" min="1" :class="{ 'is-invalid': errors.intervalUnosaIzmena }" v-numericOnly />
                                          <span v-if="errors.intervalUnosaIzmena" class="invalid-feedback">{{ intervalIzmenaPoruka }}</span>
                                      </div>
                                      <div class="mb-3">
                                          <label class="form-label">Datum do kada se vrši automatski unos:</label>
                                          <date-picker v-model="datumUnosaIzmena" format="DD.MM.YYYY."></date-picker><br />
                                          <small class="form-text text-muted">Napomena: Ako je polje prazno, automatski unos nema kraj.</small>
                                      </div>
                                      <h5>Lista obrada za automatski unos:</h5>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="obradaTLDIzmena" v-model="obradaTLDIzmena" />
                                              <label class="form-check-label" for="obradaTLDIzmena">Obrada po TLD</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="odbaceniDomeniIzmena" v-model="odbaceniDomeniIzmena" />
                                              <label class="form-check-label" for="odbaceniDomeniIzmena">Obrada po odbačenim domenima</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="kljucneReciIzmena" v-model="kljucneReciIzmena" />
                                              <label class="form-check-label" for="kljucneReciIzmena">Obrada po ključnim recima</label>
                                          </div>
                                      </div>
                                      <div class="mb-3">
                                          <div class="form-check">
                                              <input class="form-check-input" type="checkbox" id="odbaceneEmailIzmena" v-model="odbaceneEmailIzmena" />
                                              <label class="form-check-label" for="odbaceneEmailIzmena">Odbačene email adrese</label>
                                          </div>
                                      </div>
                                  </div>
                              </form>
                          </div>
                          <div class="modal-footer">
                              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Odustani</button>
                              <button @click="izmeniIzvor" id="izmeniIzvor" type="button" class="btn btn-primary">Sačuvaj</button>
                          </div>
                      </div>
                  </div>
              </div>

              <!--Arhiviranje izvora-->
              <div class="modal fade" id="archiveModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                  <div class="modal-dialog modal-sm">
                      <div class="modal-content">
                          <div class="modal-header">
                              <h5 class="modal-title" id="exampleModalLabel">Arhiviranje Izvora</h5>
                              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                          </div>
                          <div class="modal-body m-2">
                              <div class="mb-3">
                                  <div class="form-check">
                                      <input class="form-check-input" type="radio" name="archive" value="arhiviraj" id="arhiviraj" v-model="selectArhiviraj" />
                                      <label class="form-check-label" for="arhiviraj"> Arhiviraj </label>
                                  </div>
                                  <br />
                                  <div class="form-check">
                                      <input class="form-check-input" type="radio" name="archive" value="dearhiviraj" id="dearhiviraj" v-model="selectArhiviraj" />
                                      <label class="form-check-label" for="dearhiviraj"> Dearhiviraj </label>
                                  </div>
                              </div>
                              <br />
                          </div>

                          <div class="modal-footer">
                              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Odustani</button>
                              <button @click="arhivirajIzvor" type="button" class="btn btn-primary">Sačuvaj</button>
                          </div>
                      </div>
                  </div>
              </div>

              <!--Izvoz email adresa-->
              <div class="modal fade" id="izvozEmailModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                  <div class="modal-dialog">
                      <div class="modal-content">
                          <div class="modal-header">
                              <h5 class="modal-title" id="exampleModalLabel">Izvoz E-mail Adresa</h5>
                              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                          </div>
                          <div class="modal-body m-2">
                              <div class="mb-3">
                                  <h5>
                                      Ukupno adresa vezano za izvor: <b>{{ this.numberOfSourcesInIzvori }}</b>
                                  </h5>
                                  <br />
                              </div>

                              <label class="form-label required">Izaberite oblik izvoza e-mail adresa: </label>
                              <div class="mb-3">
                                  <div class="form-check">
                                      <input class="form-check-input" type="radio" name="exportType" value="txt" id="txt" v-model="selectExportType" />
                                      <label class="form-check-label" for="txt"> Text (.txt) </label>
                                  </div>
                                  <div class="form-check">
                                      <input class="form-check-input" type="radio" name="exportType" value="excel" id="excel" v-model="selectExportType" />
                                      <label class="form-check-label" for="excel"> Excel (.csv) </label>
                                  </div>
                              </div>
                              <br />

                              <!--Odabir Kolona Za Excel Export-->
                              <div v-if="selectExportType === 'excel'">
                                  <h5>Odaberite kolone za izvoz:</h5>
                                  <div class="row mt-3">
                                      <div class="col-6">
                                          <div class="mb-3">
                                              <div class="form-check">
                                                  <input class="form-check-input" type="checkbox" id="addressesId" v-model="addressesId" />
                                                  <label class="form-check-label" for="addressesId">ID</label>
                                              </div>
                                          </div>
                                          <div class="mb-3">
                                              <div class="form-check">
                                                  <input class="form-check-input" type="checkbox" id="firstName" v-model="firstName" />
                                                  <label class="form-check-label" for="firstName">Ime</label>
                                              </div>
                                          </div>
                                          <div class="mb-3">
                                              <div class="form-check">
                                                  <input class="form-check-input" type="checkbox" id="lastName" v-model="lastName" />
                                                  <label class="form-check-label" for="lastName">Prezime</label>
                                              </div>
                                          </div>
                                      </div>
                                      <div class="col-6">
                                          <div class="mb-3">
                                              <div class="form-check">
                                                  <input class="form-check-input" type="checkbox" id="email" v-model="email" />
                                                  <label class="form-check-label" for="email">E-mail</label>
                                              </div>
                                          </div>
                                          <div class="mb-3">
                                              <div class="form-check">
                                                  <input class="form-check-input" type="checkbox" id="inputType" v-model="inputType" />
                                                  <label class="form-check-label" for="inputType">Tip Unosa</label>
                                              </div>
                                          </div>
                                          <div class="mb-3">
                                              <div class="form-check">
                                                  <input class="form-check-input" type="checkbox" id="inputDate" v-model="inputDate" />
                                                  <label class="form-check-label" for="inputDate">Datum Unosa</label>
                                              </div>
                                          </div>
                                      </div>
                                  </div>
                              </div>
                          </div>

                          <div class="modal-footer">
                              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Odustani</button>
                              <button @click="exportIzvor" type="button" class="btn btn-primary">Izvezite</button>
                          </div>
                      </div>
                  </div>
              </div>
          </div>
      </template>

      <script>
      var csrfToken = $('meta[name="csrf-token"]').attr("content");
      export default {
          directives: {
              numericOnly: {
                  inserted: function (el) {
                      el.addEventListener("input", function (event) {
                          const inputValue = event.target.value;
                          const numericValue = inputValue.replace(/\D/g, "");
                          event.target.value = numericValue;
                      });
                  },
              },
          },
          components: { DatePicker },
          props: {
              id: {
                  type: Number,
                  required: true,
              },
              user_privilege: {
                  type: Object,
                  required: true,
              },
          },
          data() {
              return {
                  nazivIzvora: "",
                  opisIzvora: "",
                  nacinUlaska: "",
                  selectedNacinUlaska: null,
                  errors: {},
                  groupID: "",
                  datumObrade: new Date(),
                  datumUbacivanja: new Date(),
                  obradaTLD: false,
                  odbaceniDomeni: false,
                  kljucneReci: false,
                  odbaceneEmail: false,
                  apiRuta: "",
                  intervalUnosa: "",
                  datumUnosa: null,

                  izvorID: null,
                  statusIzvora: "",
                  nazivIzmeni: "",
                  selectedNacinUlaskaIzmena: null,
                  obradaTLDIzmena: false,
                  odbaceniDomeniIzmena: false,
                  kljucneReciIzmena: false,
                  odbaceneEmailIzmena: false,
                  apiRutaIzmena: "",
                  intervalUnosaIzmena: "",
                  datumUnosaIzmena: null,

                  selectArhiviraj: null,

                  datumUbacivanjaFilter: null,
                  datumObradeFilter: null,
                  nacinUlaskaFilter: null,
                  statusIzvoraFilter: null,

                  numberOfSourcesInIzvori: null,
                  poruka: "",
                  intervalIzmenaPoruka: "",

                  selectExportType: null,
                  addressesId: true,
                  firstName: true,
                  lastName: true,
                  email: true,
                  inputType: true,
                  inputDate: true,
              };
          },
          methods: {
              errorsClean() {
                  this.errors = {};
                  this.selectedNacinUlaska = "";
                  this.obradaTLD = "";
                  this.odbaceniDomeni = "";
                  this.kljucneReci = "";
                  this.odbaceneEmail = "";
                  this.apiRuta = "";
                  this.intervalUnosa = "";
                  this.datumUnosa = "";
                  this.selectExportType = "";
                  this.addressesId = true;
                  this.firstName = true;
                  this.lastName = true;
                  this.email = true;
                  this.inputType = true;
                  this.inputDate = true;
              },

              dodajIzvor() {
                  let modal = $("#createIzvor");
                  let th = this;
                  this.errors = {};
                  if (this.obradaTLD === "") {
                      this.obradaTLD = false;
                  }
                  if (this.odbaceniDomeni === "") {
                      this.odbaceniDomeni = false;
                  }
                  if (this.kljucneReci === "") {
                      this.kljucneReci = false;
                  }
                  if (this.odbaceneEmail === "") {
                      this.odbaceneEmail = false;
                  }

                  if (this.nazivIzvora === "") {
                      this.errors.nazivIzvora = true;
                      this.nazivPoruka = "Molimo Vas unesite naziv izvora.";
                  }

                  const regexNaziv = /^[A-Za-z0-9\s]*$/;
                  if (!regexNaziv.test(this.nazivIzvora)) {
                      this.errors.nazivIzvora = true;

                      this.nazivPoruka = "Netačan format naziva izvora.";
                  }

                  if (this.opisIzvora === "") {
                      this.errors.opisIzvora = true;
                      this.opisPoruka = "Molimo Vas unesite opis izvora.";
                  }
                  const regexOpis = /^[A-Za-z0-9\sšćžđč]*$/;
                  if (!regexOpis.test(this.opisIzvora)) {
                      this.errors.opisIzvora = true;

                      this.opisPoruka = "Netačan format opisa izvora.";
                  }

                  if (this.selectedNacinUlaska === "") {
                      Swal.fire({
                          icon: "warning",
                          title: "Greska!",
                          text: "Morate izabrati način unosa.",
                          customClass: {
                              confirmButton: "confirmationBtn",
                          },
                      });

                      return;
                  }
                  if (this.selectedNacinUlaska === "automatski") {
                      if (this.apiRuta === "") {
                          this.errors.apiRuta = true;
                          this.apiPoruka = "Molimo Vas unesite API rutu.";
                          return;
                      }
                      const regexAPI = /^(http(s)?:\/\/(www.)?[a-z]{1,20}(\.[a-z]{1,20})?\.[a-z]{2,6}(\/)?([a-z]{1,20})?)*$/;
                      if (!regexAPI.test(this.apiRuta)) {
                          this.errors.apiRuta = true;

                          this.apiPoruka = "Netačan format API rute.";
                          return;
                      }

                      if (this.intervalUnosa === "") {
                          this.errors.intervalUnosa = true;
                          this.poruka = "Molimo Vas unesite interval unosa.";
                          return;
                      }
                      const regex = /^[1-9][0-9]*$/;
                      if (!regex.test(this.intervalUnosa)) {
                          this.errors.intervalUnosa = true;

                          this.poruka = "Interval unosa ne sme počinjati sa nulom.";
                          return;
                      }
                  }
                  if (Object.keys(this.errors).length > 0) {
                      return;
                  }

                  const izvorData = {
                      nazivIzvora: this.nazivIzvora,
                      opisIzvora: this.opisIzvora,
                      selectedNacinUlaska: this.selectedNacinUlaska,
                      groupID: th.id,
                      datumObrade: this.datumObrade,
                      datumUbacivanja: this.datumUbacivanja,
                      obradaTLD: this.obradaTLD,
                      odbaceniDomeni: this.odbaceniDomeni,
                      kljucneReci: this.kljucneReci,
                      odbaceneEmail: this.odbaceneEmail,
                      apiRuta: this.apiRuta,
                      intervalUnosa: this.intervalUnosa,
                      datumUnosa: this.datumUnosa,
                  };
                  console.log(izvorData);
                  $("#dodajIzvor").prop("disabled", true);
                  axios
                      .post("/administracija-grupa/dodajIzvor", izvorData, {
                          headers: {
                              "X-CSRF-TOKEN": csrfToken,
                          },
                      })
                      .then((response) => {
                          console.log(response.data);
                          $("#dodajIzvor").prop("disabled", false);
                          Swal.fire({
                              icon: "success",
                              text: "Uspešno sačuvano",
                              timer: 5000,
                              customClass: {
                                  confirmButton: "confirmationBtn",
                              },
                          });
                          modal.modal("hide"),
                              (this.nazivIzvora = ""),
                              (this.opisIzvora = ""),
                              (this.selectedNacinUlaska = ""),
                              (this.obradaTLD = ""),
                              (this.odbaceniDomeni = ""),
                              (this.kljucneReci = ""),
                              (this.odbaceneEmail = ""),
                              (this.apiRuta = ""),
                              (this.intervalUnosa = ""),
                              (this.datumUnosa = ""),
                              $("#izvorTabela").DataTable().ajax.reload();
                      })
                      .catch((error) => {
                          $("#dodajIzvor").prop("disabled", false);
                          if (error.response.status == 401 || error.response.status == 419) {
                              window.location.replace("/login");
                          }
                          Swal.fire({
                              icon: "warning",
                              text: "Greška prilikom kreiranja izvora!",
                              customClass: {
                                  confirmButton: "confirmationBtn",
                              },
                          });
                          console.error(error);
                      });
              },

              podaciIzvor() {
                  let th = this;
                  this.izvorDataTable();
                  $(document).on("click", ".izmeniAkcija", function (e) {
                      th.izvorID = $(this).data("entry-id");
                      console.log(th.izvorID);

                      const podaci = {
                          izvorID: th.izvorID,
                      };
                      axios
                          .post("/administracija-grupa/podaciIzvor", podaci, {
                              headers: {
                                  "X-CSRF-TOKEN": csrfToken,
                              },
                          })
                          .then((response) => {
                              th.errors = {};
                              console.log(response.data);

                              th.statusIzvora = response.data.selectedData[0].statusIzvora;
                              th.nazivIzmeni = response.data.selectedData[0].nazivIzvora;
                              th.selectedNacinUlaskaIzmena = response.data.selectedData[0].nacinUlaska;
                              if (response.data.selectedData[0].nacinUlaska === "Ručno") {
                                  document.getElementById("rucnoIzmena").checked = true;
                              } else if (response.data.selectedData[0].nacinUlaska === "Automatski") {
                                  document.getElementById("automatskiIzmena").checked = true;
                              }
                              th.obradaTLDIzmena = response.data.selectedData[0].obrada_po_tld;
                              th.odbaceniDomeniIzmena = response.data.selectedData[0].obrada_po_odbacenim_domenima;
                              th.kljucneReciIzmena = response.data.selectedData[0].obrada_po_kljucnim_recima;
                              th.odbaceneEmailIzmena = response.data.selectedData[0].odbacene_email_adrese;
                              th.apiRutaIzmena = response.data.selectedData[0].apiRuta;
                              th.intervalUnosaIzmena = response.data.selectedData[0].interval_unosa;
                              th.datumUnosaIzmena = response.data.selectedData[0].datum_unosa ? new Date(response.data.selectedData[0].datum_unosa) : null;
                          })
                          .catch((error) => {
                              if (error.response.status == 401 || error.response.status == 419) {
                                  window.location.replace("/login");
                              }
                              console.error(error);
                          });
                  });
              },

              izmeniIzvor() {
                  let modal = $("#izmeniIzvor");
                  let th = this;
                  this.errors = {};

                  if (this.nazivIzmeni === "") {
                      this.errors.nazivIzmeni = true;
                      this.nazivIzmeniPoruka = "Molimo Vas unesite naziv izvora.";
                  }

                  const regexNazivIzmeni = /^[A-Za-z0-9\s]*$/;
                  if (!regexNazivIzmeni.test(this.nazivIzmeni)) {
                      this.errors.nazivIzmeni = true;

                      this.nazivIzmeniPoruka = "Netačan format naziva izvora.";
                  }

                  if (this.nazivIzmeni === "") {
                      this.errors.nazivIzmeni = true;
                  }
                  if (this.obradaTLDIzmena === "") {
                      this.obradaTLDIzmena = false;
                  }
                  if (this.odbaceniDomeniIzmena === "") {
                      this.odbaceniDomeniIzmena = false;
                  }
                  if (this.kljucneReciIzmena === "") {
                      this.kljucneReciIzmena = false;
                  }
                  if (this.odbaceneEmailIzmena === "") {
                      this.odbaceneEmail = false;
                  }
                  if (this.selectedNacinUlaskaIzmena === "automatski") {
                      if (this.apiRutaIzmena === null || this.apiRutaIzmena === "") {
                          this.errors.apiRutaIzmena = true;
                          this.apiPorukaIzmena = "Molimo Vas unesite API rutu.";
                          return;
                      }
                      const regexAPIIzmena = /^(http(s)?:\/\/(www.)?[a-z]{1,20}(\.[a-z]{1,20})?\.[a-z]{2,6}(\/)?([a-z]{1,20})?)*$/;
                      if (!regexAPIIzmena.test(this.apiRutaIzmena)) {
                          this.errors.apiRutaIzmena = true;
                          this.apiPorukaIzmena = "Netačan format API rute.";
                          return;
                      }
                      if (this.intervalUnosaIzmena === null || this.intervalUnosaIzmena === "") {
                          this.errors.intervalUnosaIzmena = true;
                          this.intervalIzmenaPoruka = "Molimo Vas unesite interval unosa.";
                          return;
                      }
                      const regex = /^[1-9][0-9]*$/;
                      if (!regex.test(this.intervalUnosaIzmena)) {
                          this.errors.intervalUnosaIzmena = true;

                          this.intervalIzmenaPoruka = "Interval unosa ne sme počinjati sa nulom.";
                          return;
                      }
                  }

                  if (this.selectedNacinUlaskaIzmena === "rucno") {
                      this.apiRutaIzmena = null;
                      this.intervalUnosaIzmena = null;
                      this.datumUnosaIzmena = null;
                  }
                  if (Object.keys(this.errors).length > 0) {
                      return;
                  }

                  const izmeniPodaci = {
                      groupID: th.id,
                      izvorID: th.izvorID,
                      nazivIzmeni: th.nazivIzmeni,
                      statusIzvora: th.statusIzvora,
                      selectedNacinUlaskaIzmena: th.selectedNacinUlaskaIzmena,
                      obradaTLDIzmena: th.obradaTLDIzmena,
                      odbaceniDomeniIzmena: th.odbaceniDomeniIzmena,
                      kljucneReciIzmena: th.kljucneReciIzmena,
                      odbaceneEmailIzmena: th.odbaceneEmailIzmena,
                      apiRutaIzmena: th.apiRutaIzmena,
                      intervalUnosaIzmena: th.intervalUnosaIzmena,
                      datumUnosaIzmena: th.datumUnosaIzmena,
                  };
                  $("#izmeniIzvor").prop("disabled", true);
                  axios
                      .post("/administracija-grupa/izmeniIzvor", izmeniPodaci, {
                          headers: {
                              "X-CSRF-TOKEN": csrfToken,
                          },
                      })
                      .then((response) => {
                          console.log(response.data);
                          $("#izmeniIzvor").prop("disabled", false);
                          Swal.fire({
                              icon: "success",
                              text: "Uspešno sačuvano",
                              timer: 5000,
                              customClass: {
                                  confirmButton: "confirmationBtn",
                              },
                          });
                          modal.modal("hide");

                          $("#izvorTabela").DataTable().ajax.reload();
                      })
                      .catch((error) => {
                          $("#izmeniIzvor").prop("disabled", false);
                          if (error.response.status == 401 || error.response.status == 419) {
                              window.location.replace("/login");
                          }
                          console.error(error);
                      });
              },

              selectArchive() {
                  let th = this;
                  this.izvorDataTable();
                  $(document).on("click", ".archiveAkcija", function (e) {
                      th.izvorID = $(this).data("entry-id");
                      console.log(th.izvorID);

                      const podaci = {
                          izvorID: th.izvorID,
                      };

                      axios
                          .post("/administracija-grupa/selectArchive", podaci, {
                              headers: {
                                  "X-CSRF-TOKEN": csrfToken,
                              },
                          })
                          .then((response) => {
                              console.log(response.data);

                              th.selectArhiviraj = response.data.selectedData[0].arhiviraj;
                              if (response.data.selectedData[0].arhiviraj === "Arhiviraj") {
                                  document.getElementById("arhiviraj").checked = true;
                              } else if (response.data.selectedData[0].arhiviraj === "Dearhiviraj") {
                                  document.getElementById("dearhiviraj").checked = true;
                              }
                          })
                          .catch((error) => {
                              if (error.response.status == 401 || error.response.status == 419) {
                                  window.location.replace("/login");
                              }
                              console.error(error);
                          });
                  });
              },

              arhivirajIzvor() {
                  let modal = $("#archiveModal");
                  let th = this;
                  const podaci = {
                      groupID: th.id,
                      izvorID: th.izvorID,
                      selectArhiviraj: th.selectArhiviraj,
                  };

                  axios
                      .post("/administracija-grupa/archive", podaci, {
                          headers: {
                              "X-CSRF-TOKEN": csrfToken,
                          },
                      })
                      .then((response) => {
                          console.log(response.data);
                          Swal.fire({
                              icon: "success",
                              text: "Uspešno sačuvano",
                              timer: 5000,
                              customClass: {
                                  confirmButton: "confirmationBtn",
                              },
                          });
                          modal.modal("hide");

                          $("#izvorTabela").DataTable().ajax.reload();
                      })
                      .catch((error) => {
                          if (error.response.status == 401 || error.response.status == 419) {
                              window.location.replace("/login");
                          }
                          console.error(error);
                      });
              },

              formatDatumObrade() {
                  if (this.datumObradeFilter != null) {
                      return moment(this.datumObradeFilter).format("YYYY-MM-DD");
                  }
                  return null;
              },
              formatDatumUbacivanja() {
                  if (this.datumUbacivanjaFilter != null) {
                      return moment(this.datumUbacivanjaFilter).format("YYYY-MM-DD");
                  }
                  return null;
              },

              clearFilter() {
                  this.datumObradeFilter = null;
                  this.datumUbacivanjaFilter = null;
                  this.statusIzvoraFilter = "";
                  this.nacinUlaskaFilter = "";

                  this.izvorDataTable();
              },

              numberOfSourcesForExport() {
                  let th = this;
                  this.izvorDataTable();
                  $(document).on("click", ".izvozAkcija", function (e) {
                      th.izvorID = $(this).data("entry-id");
                      //console.log(th.izvorID)
                      const podaci = {
                          izvorID: th.izvorID,
                      };
                      axios
                          .post("/administracija-grupa/izvoriExport/numberOfSourcesInIzvori", podaci, {
                              headers: {
                                  "X-CSRF-TOKEN": csrfToken,
                              },
                          })
                          .then((response) => {
                              th.numberOfSourcesInIzvori = response.data;
                          })
                          .catch((error) => {
                              if (error.response.status == 401 || error.response.status == 419) {
                                  window.location.replace("/login");
                              }
                              console.error(error);
                          });
                  });
              },

              exportIzvor() {
                  let modal = $("#izvozEmailModal");
                  let th = this;

                  if (this.selectExportType == "txt") {
                      axios
                          .post("/administracija-grupa/izvoriTxtExport", { izvorID: th.izvorID, groupID: th.id }, {})
                          .then((response) => {
                              modal.modal("hide");
                              th.errorsClean();
                              const blob = new Blob([response.data], { type: "text/plain" });
                              const url = URL.createObjectURL(blob);
                              const link = document.createElement("a");
                              link.href = url;
                              link.setAttribute("download", "adrese.txt");
                              document.body.appendChild(link);
                              link.click();
                              document.body.removeChild(link);
                          })
                          .catch((error) => {
                              if (error.response.data.error == "empty") {
                                  Swal.fire({
                                      icon: "warning",
                                      text: "Fajl koji želite da izvezete je prazan!",
                                      customClass: {
                                          confirmButton: "confirmationBtn",
                                      },
                                  });
                                  return;
                              }

                              Swal.fire({
                                  icon: "warning",
                                  text: "Greška prilikom izvoza sadržaja izvora!",
                                  customClass: {
                                      confirmButton: "confirmationBtn",
                                  },
                              });
                              console.error(error);
                          });
                  }

                  if (this.selectExportType === "excel") {
                      axios
                          .get("/administracija-grupa/izvoriExport", {
                              params: {
                                  addressesId: th.addressesId,
                                  firstName: th.firstName,
                                  lastName: th.lastName,
                                  email: th.email,
                                  inputType: th.inputType,
                                  inputDate: th.inputDate,
                                  id: th.izvorID,
                                  groupID: th.id,
                              },
                          })
                          .then((response) => {
                              modal.modal("hide");
                              th.errorsClean();
                              const url = window.URL.createObjectURL(new Blob([response.data]));
                              const link = document.createElement("a");
                              link.href = url;
                              link.setAttribute("download", "izvor_" + th.izvorID + ".csv");
                              document.body.appendChild(link);
                              link.click();
                          })
                          .catch((error) => {
                              if (error.response.data.error == "empty") {
                                  Swal.fire({
                                      icon: "warning",
                                      text: "Morate izabrati barem jedno polje za izvoz!",
                                      customClass: {
                                          confirmButton: "confirmationBtn",
                                      },
                                  });
                                  return;
                              }
                              Swal.fire({
                                  icon: "warning",
                                  text: "Greška prilikom izvoza sadržaja izvora!",
                                  customClass: {
                                      confirmButton: "confirmationBtn",
                                  },
                              });
                              console.error(error);
                          });
                  }
              },
              izvorDataTable() {
                  var th = this;
                  $("#izvorTabela").DataTable().clear().draw();
                  $("#izvorTabela").DataTable().clear().destroy();
                  var table = $("#izvorTabela").DataTable({
                      oLanguage: {
                          oPaginate: {
                              sNext: "Sledeća",
                              sPrevious: "Prethodna",
                          },
                          sEmptyTable: "Nema podataka.",
                          sLengthMenu: "Prikaz _MENU_ izvora",
                          sZeroRecords: "Nema pronađenih unosa!",
                          sInfo: "Prikazano _START_ do _END_ od _TOTAL_ unosa",
                          sInfoEmpty: "Nema unosa",
                          sInfoFiltered: "(filtrirano od_MAX_unosa)",
                          sSearch: "Pretraga:",
                          sProcessing: "Molimo sačekajte ...",
                      },
                      processing: true,
                      serverSide: true,
                      destroy: true,
                      ajax: {
                          url: "/administracija-grupa/izvoriDataTable",
                          type: "POST",
                          error: function (xhr, textStatus, errorThrown) {
                              if (xhr.status === 401 || xhr.status === 419) {
                                  window.location.replace("/login");
                              }
                          },
                          data: {
                              groupID: th.id,
                              datumObradeFilter: th.formatDatumObrade(),
                              datumUbacivanjaFilter: th.formatDatumUbacivanja(),
                              statusIzvoraFilter: th.statusIzvoraFilter,
                              nacinUlaskaFilter: th.nacinUlaskaFilter,
                          },
                          headers: {
                              "X-CSRF-TOKEN": $('meta[name="csrf-token"]').attr("content"),
                          },
                      },
                      columnDefs: [
                          { targets: 0, orderable: true, width: "20px" },
                          { targets: 1, orderable: true, width: "10%" },
                          { targets: 2, orderable: true },
                          { targets: 3, orderable: false, width: "20px" },
                          { targets: 4, orderable: false, width: "100px" },
                          { targets: 5, orderable: false, width: "20px" },
                          { targets: 6, orderable: false, width: "10%" },
                          { targets: 7, orderable: false, width: "10%" },
                          { targets: 8, orderable: false },
                      ],
                      columns: [
                          { data: "izvorID" },
                          {
                              data: "nazivIzvora",
                              render: function (data, type, row) {
                                  return '<a type="button" class="link-primary" href="/administracija-grupa/izvor/' + th.id + "/addresses/" + row.izvorID + '">' + data + "</a>";
                              },
                          },
                          { data: "opisIzvora" },
                          {
                              data: "statusIzvora",
                              render: function (data, type, row) {
                                  if (data === "Neaktivan") {
                                      return '<span class="neaktivan-izvor">' + data + "</span>";
                                  } else {
                                      return data;
                                  }
                              },
                              createdCell: function (cell, cellData, rowData, rowIndex, colIndex) {
                                  if (cellData === "Neaktivan") {
                                      $(cell).parent().addClass("neaktivan-izvor");
                                  }
                              },
                          },
                          { data: "brojEmail" },
                          { data: "nacinUlaska" },
                          {
                              data: "datumObrade",
                              render: function (data) {
                                  return moment(data).format("DD.MM.Y. HH:mm");
                              },
                          },
                          {
                              data: "datumUbacivanja",
                              render: function (data) {
                                  return moment(data).format("DD.MM.Y. HH:mm");
                              },
                          },
                          {
                              data: "akcije",
                              render: function (data, type, row) {
                                  if (th.user_privilege["role"] == "admin" || th.user_privilege["source_export_privilege"] == 1) {
                                      return (
                                          '<div class="dropdown">' +
                                          '<button type="button" class="btn btn-primary dropdown-toggle float-end" data-bs-toggle="dropdown">Akcije</button>' +
                                          '<div class="dropdown-menu">' +
                                          '<a type="button" data-bs-toggle="modal" data-bs-target="#izmeniIzvor" class="izmeniAkcija dropdown-item" data-entry-id="' +
                                          row.izvorID +
                                          '">Izmena</a>' +
                                          '<a type="button" data-bs-toggle="modal" data-bs-target="#archiveModal" class="archiveAkcija dropdown-item" data-entry-id="' +
                                          row.izvorID +
                                          '">Arhiviraj Izvor</a>' +
                                          '<a type="button" data-bs-toggle="modal" data-bs-target="#izvozEmailModal" class="izvozAkcija dropdown-item" data-entry-id="' +
                                          row.izvorID +
                                          '">Izvoz Email Adresa</a>' +
                                          "</div>" +
                                          "</div>"
                                      );
                                  } else {
                                      return (
                                          '<div class="dropdown">' +
                                          '<button type="button" class="btn btn-primary dropdown-toggle float-end" data-bs-toggle="dropdown">Akcije</button>' +
                                          '<div class="dropdown-menu">' +
                                          '<a type="button" data-bs-toggle="modal" data-bs-target="#izmeniIzvor" class="izmeniAkcija dropdown-item" data-entry-id="' +
                                          row.izvorID +
                                          '">Izmena</a>' +
                                          '<a type="button" data-bs-toggle="modal" data-bs-target="#archiveModal" class="archiveAkcija dropdown-item" data-entry-id="' +
                                          row.izvorID +
                                          '">Arhiviraj Izvor</a>' +
                                          "</div>" +
                                          "</div>"
                                      );
                                  }
                              },
                          },
                      ],
                  });
              },
          },
          mounted() {
              this.izvorDataTable();
              this.podaciIzvor();
              this.izmeniIzvor();
              this.selectArchive();
              this.numberOfSourcesForExport();
          },
      };
      </script>
      <style>
      .neaktivan-izvor td {
          background-color: #fdecec;
          /* Blago roze boja */
          color: red !important;
      }

      /*.table-striped>tbody>tr:nth-child(odd)>td,
      .table-striped>tbody>tr:nth-child(odd)>th {
         background-color: gray;
       }*/
      .form-check {
          display: inline-block;
          margin-right: 20px;
      }
      </style>
