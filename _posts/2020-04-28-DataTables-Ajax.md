---
layout: post
title: Ajax ile DataTables eklentisini GET yöntemiyle veri yükleme
---  
## DataTables Nedir ? ##
Web uygulamalarında, veri gösteriminde kullanabileceğiniz  önemli bir JQuery eklentisidir.  
Genelde tablo ağırlıklı uygulamalarda tercih ettiğimiz bir eklenti olan DataTables yeri geldiğinde işimizi çok kolaylaştırmıştır.  
## Ajax ile DataTables veri yükleme ##
Yazacağımız Script kodu ile DataTables eklentimizi GET yöntemiyle alınan veriler ile dolduracağız.  

```javascript 
 <script type="text/javascript">
        $(document).ready(function () {
            var table = $("#myTable tbody");
            $.ajax({
                url: '/User/GetAllUser',
                method: "GET",
                xhrFields: {
                    withCredentials:true
                },
                success: function (data) {
                    table.empty();
                    $.each(data, function (a, b) {
                        table.append("<tr><td>"+b.CannonicalName+"</td>"+
                            "<td>"+b.SamAccountName+"</td>"+
                             "<td>"+b.UserAccountCode+"</td>"+
                              "<td>"+b.UserAccountControl+"</td>"+
                               "<td>"+b.LastLogon+"</td>"+
                                "<td>"+b.WhenCreated+"</td>"+
                                 "<td>"+b.PwdLastSet+"</td>"+
                                 "<td> <button class='btn btn-success'>Yetki</button></td></tr>");
                    });

                    $("#myTable").DataTable();
                    }
            })
        });
                          
    </script>
    
    ```
    
    tablomuza ait Html kodu ise şu şekilde olmalıdır :  
    
    ``` html 
    
      <table id="myTable" class="display" >
                   
                        <thead>
                            <tr>
                                <th>Adı</th>
                                <th>Kullanıcı Adı</th>
                                <th>Durum Kodu</th>
                                <th>Durumu</th>
                                <th>Son Bağlantı</th>
                                <th>Oluşturulma Tarihi</th>
                                <th>Son Parola Değiştirme</th>
                                <th>Yetki</th>
                              

                            </tr>
                        </thead>
                        <tfoot>
                            <tr>
                                <th>Adı</th>
                                <th>Kullanıcı Adı</th>
                                <th>Durum Kodu</th>
                                <th>Durumu</th>
                                <th>Son Bağlantı</th>
                                <th>Oluşturulma Tarihi</th>
                                <th>Son Parola Değiştirme</th>
                                <th>Yetki</th>
                               
                            </tr>
                        </tfoot>
                    <tbody>


                    </tbody>
                      
                    </table>
    
    ```

## Sunucu Tarafı ##
Arka yüzde yapılacak işlem çok daha kolaydır. Veriyi Json olarak döndürecek bir method yazmak yeterli olacaktır.  

```c#
 public JsonResult GetAllUser()
        {
            
            var liste = KullaniciGetir(); //UserEntity sınıfından oluşan verilerin listesini döndüren method
            return Json(liste, JsonRequestBehavior.AllowGet);


        }

 public class UserEntities
    {

        public string CannonicalName { get; set; }
        public string SamAccountName { get; set; }
        public string UserAccountCode { get; set; }
        public string UserAccountControl { get; set; }
        public string LastLogon { get; set; }
        public string WhenCreated { get; set; }
        public string PwdLastSet { get; set; }

    }

```

 

 Hepsi bu kadar. Sağlıcakla kalın 



