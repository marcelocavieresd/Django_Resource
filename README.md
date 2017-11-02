# Django_Resource


#Función Get Session

def get_session(request):
    if request.GET["token"]:
        token = request.GET["token"].replace(" ","+")
        cipher = AES.new("bdc635k2-283d-4a2c-a477-339ea866",AES.MODE_ECB)
        decoded = cipher.decrypt(base64.b64decode(str(token)))
        decoded = decoded.split("-")
        post_data = {"token":decoded[0]}
        header = {"Content-Type":'application/x-www-form-urlencoded'}
        response2 = requests.post('http://cubesoa.asuscomm.com:90/get/get_session/',data=post_data,headers=header)
        respuesta = ast.literal_eval(response2.content)
        print respuesta
        request.session["name"] = respuesta["name"]
        request.session["token"] = respuesta["token"]
        request.session["rut"] = respuesta["rut"]
        request.session["type"] = respuesta["type"]
        request.session["username"] = respuesta["username"]
        #de aqui en adelante implementar lo necesario para su login
        user = User_Profile.objects.filter(rut=respuesta["rut"])
        if len(user) != 1:
            user = User_Profile.objects.create(rut=respuesta["rut"],
                                              nombre=request.session["name"])
            user.save()
        return HttpResponseRedirect(reverse_lazy('index'))
    return HttpResponseRedirect(reverse_lazy('login'))
    
 #Para encriptar se debe instalar pycrypto
 pip install pycrypto
 
