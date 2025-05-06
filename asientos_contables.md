
# Mapeo contable consignación

| Etapa / Acción                         | Cuenta contable                                               | Débito | Crédito |
|---------------------------------------|----------------------------------------------------------------|--------|---------|
| Ingreso mercancía (recepción)         | 8135 - Mercancías en consignación (control deudor)            | ✔️     |         |
| Entrada de consignación               | 8235 - Contrapartida de mercancías en consignación (control acreedor) |        | ✔️      |
|                                       |                                                                |        |         |
| WH/Out remisión al cliente            | 8136 - Mercancías en consignación pendientes por facturar al cliente | ✔️     |         |
|                                       | 8135 - Mercancías en consignación                              |        | ✔️      |
| Facturación cliente              | 6135 - Costo de mercancías vendidas                            | ✔️     |         |
|                                       | 8136 - Mercancías en consignación pendientes por facturar      |        | ✔️      |
|                                       |                                                                |        |         |
| WH/IN Reposición                      | 8135 - Mercancías en consignación                              | ✔️     |         |
|                                       | 8235 - Contrapartida de mercancías en consignación             |        | ✔️      |
| Factura proveedor                     | 8235 - Contrapartida de mercancías en consignación             | ✔️     |         |
|                                       | 2205 - Proveedores (obligación formal)                         |        | ✔️      |
