---
title: 'Identificadores de cadenas de localización: Azure Active Directory B2C | Microsoft Docs'
description: Especifique los identificadores de una definición de contenido con un identificador api.signuporsignin en una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/03/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: cd5e543a6d5dc2b59847a78bd925be68f8cdde25
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76982864"
---
# <a name="localization-string-ids"></a>Identificadores de cadenas de localización

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

El elemento **Localization** permite la compatibilidad con varios idiomas o configuraciones regionales en la directiva para los recorridos del usuario. En este artículo se proporciona la lista de identificadores de localización que puede usar en la directiva. Para familiarizarse con la localización de la interfaz de usuario, vea [Localización](localization.md).

## <a name="sign-up-or-sign-in-page-elements"></a>Elementos de la página de registro o de inicio de sesión

Los siguientes identificadores sirven para una definición de contenido con el identificador `api.signuporsignin`.

| id | Valor predeterminado |
| -- | ------------- |
| **local_intro_email** | Inicie sesión con su cuenta existente. |
| **logonIdentifier_email** | Dirección de correo electrónico |
| **requiredField_email** | Escriba su dirección de correo electrónico. |
| **invalid_email** | Escriba una dirección de correo electrónico válida. |
| **email_pattern** | ^[a-zA-Z0-9.!#$%&’' *+/=?^_\`{\|}~-]+@[a-zA-Z0-9-]+(?:\\.[a-zA-Z0-9-]+)* $ |
| **local_intro_username** | Inicie sesión con su nombre de usuario. |
| **logonIdentifier_username** | Nombre de usuario |
| **requiredField_username** | Escriba su nombre de usuario. |
| **password** | Contraseña |
| **requiredField_password** | Escriba su contraseña. |
| **invalid_password** | La contraseña que ha escrito no tiene el formato esperado. |
| **forgotpassword_link** | ¿Ha olvidado la contraseña? |
| **createaccount_intro** | ¿No tiene una cuenta? |
| **createaccount_link** | Regístrese ahora |
| **divider_title** | O BIEN |
| **cancel_message** | El usuario ha olvidado su contraseña. |
| **button_signin** | Iniciar sesión |
| **social_intro** | Inicie sesión con su cuenta social. |
  **remember_me** |Mantenga la sesión iniciada.|
| **unknown_error** | Tenemos problemas para iniciar su sesión. Inténtelo de nuevo más tarde. |

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de registro o de inicio de sesión:

![Elementos de la experiencia de usuario de la página de registro o de inicio de sesión](./media/localization-string-ids/localization-susi.png)

El identificador de los proveedores de identidades se configura en el elemento **ClaimsExchange** del recorrido del usuario. Para localizar el título del proveedor de identidades, **ElementType** está establecido en `ClaimsProvider`, mientras que **StringId** se establece en el identificador de `ClaimsExchange`.

```XML
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
      <Value>objectId</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="MicrosoftExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccount" />
  </ClaimsExchanges>
</OrchestrationStep>
```

En el ejemplo siguiente se localiza el proveedor de identidades de Facebook en árabe:

```XML
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">فيس بوك</LocalizedString>
```

### <a name="sign-up-or-sign-in-error-messages"></a>Mensajes de error al iniciar sesión o registrarse

| id | Valor predeterminado |
| -- | ------------- |
| **UserMessageIfInvalidPassword** | La contraseña es incorrecta. |
| **UserMessageIfClaimsPrincipalDoesNotExist** | Parece que no encontramos su cuenta. |
| **UserMessageIfOldPasswordUsed** | Parece que ha usado una contraseña antigua. |
| **DefaultMessage** | Nombre de usuario o contraseña no válidos. |
| **UserMessageIfUserAccountDisabled** | Su cuenta se ha bloqueado. Contacte con la persona responsable de soporte técnico para desbloquearla y vuelva a intentarlo. |
| **UserMessageIfUserAccountLocked** | Su cuenta se bloqueó temporalmente para impedir un uso no autorizado. Vuelva a intentarlo más tarde. |
| **AADRequestsThrottled** | Hay demasiadas solicitudes en este momento. Espere un poco y vuelva a intentarlo. |

## <a name="sign-up-and-self-asserted-pages-user-interface-elements"></a>Elementos de la interfaz de usuario de páginas autoafirmadas o de registro

Los siguientes identificadores corresponden a una definición de contenido con un identificador `api.localaccountsignup` o a cualquier definición de contenido que empiece con `api.selfasserted`, como `api.selfasserted.profileupdate` y `api.localaccountpasswordreset`.

| id | Valor predeterminado |
| -- | ------------- |
| **ver_sent** | Se ha enviado el código de verificación a: |
| **ver_but_default** | Valor predeterminado |
| **cancel_message** | El usuario ha cancelado la introducción de información afirmada por él mismo. |
| **preloader_alt** | Espere |
| **ver_but_send** | Envíe un código de comprobación. |
| **alert_yes** | Sí |
| **error_fieldIncorrect** | Un campo o varios se han rellenado incorrectamente. Compruebe las entradas y vuelva a intentarlo. |
| **year** | Year |
| **verifying_blurb** | Espere mientras se procesa la información. |
| **button_cancel** | Cancelar |
| **ver_fail_no_retry** | Ha hecho demasiados intentos incorrectos. Inténtelo de nuevo más tarde. |
| **month** | Month |
| **ver_success_msg** | Se ha verificado la dirección de correo electrónico. Ahora puede continuar. |
| **months** | Enero, febrero, marzo, abril, mayo, junio, julio, agosto, septiembre, octubre, noviembre, diciembre |
| **ver_fail_server** | Estamos teniendo problemas para comprobar su dirección de correo electrónico. Escriba una dirección de correo electrónico válida e inténtelo de nuevo. |
| **error_requiredFieldMissing** | Falta un campo obligatorio. Rellene todos los campos obligatorios y vuelva a intentarlo. |
| **initial_intro** | Proporcione los detalles que se indican a continuación. |
| **ver_but_resend** | Envíe un código nuevo. |
| **button_continue** | Crear |
| **error_passwordEntryMismatch** | Los campos de entrada de contraseña no coinciden. Escriba la misma contraseña en ambos campos y vuelva a intentarlo. |
| **ver_incorrect_format** | Formato incorrecto. |
| **ver_but_edit** | Cambie la dirección de correo electrónico. |
| **ver_but_verify** | Compruebe el código. |
| **alert_no** | No |
| **ver_info_msg** | Se ha enviado el código de verificación a su bandeja de entrada. Cópielo en el siguiente cuadro de entrada de texto. |
| **day** | Día |
| **ver_fail_throttled** | Ha habido demasiadas solicitudes para verificar esta dirección de correo electrónico. Espere un poco y vuelva a intentarlo. |
| **helplink_text** | ¿Qué es esto? |
| **ver_fail_retry** | Este código no es correcto. Inténtelo de nuevo. |
| **alert_title** | Cancele la entrada de información. |
| **required_field** | Esta información es obligatoria. |
| **alert_message** | ¿Confirma que quiere cancelar la introducción de información? |
| **ver_intro_msg** | La comprobación es necesaria. Haga clic en el botón Enviar. |
| **ver_input** | Código de verificación |

### <a name="sign-up-and-self-asserted-pages-error-messages"></a>Mensajes de error en las páginas autoafirmadas y de registro

| id | Valor predeterminado |
| -- | ------------- |
| **UserMessageIfClaimsPrincipalAlreadyExists** | Ya existe un usuario con el identificador especificado. Elija otro. |
| **UserMessageIfClaimNotVerified** | Reclamación no comprobada: {0} |
| **UserMessageIfIncorrectPattern** | Patrón incorrecto para: {0} |
| **UserMessageIfMissingRequiredElement** | Falta un elemento obligatorio: {0} |
| **UserMessageIfValidationError** | Error en la validación de: {0} |
| **UserMessageIfInvalidInput** | {0} tiene una entrada no válida. |
| **ServiceThrottled** | Hay demasiadas solicitudes en este momento. Espere un poco y vuelva a intentarlo. |

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de registro:

![Página de registro con los nombres de los elementos de la interfaz de usuario etiquetados](./media/localization-string-ids/localization-sign-up.png)

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de registro después de que el usuario haga clic en el botón para enviar el código de verificación:

![Elementos de la experiencia de usuario al comprobar el correo electrónico en la página de registro](./media/localization-string-ids/localization-email-verification.png)


## <a name="phone-factor-authentication-page-user-interface-elements"></a>Elementos de la interfaz de usuario en la página de autenticación por teléfono

Los siguientes identificadores sirven para una definición de contenido con el identificador `api.phonefactor`.

| id | Valor predeterminado |
| -- | ------------- |
| **button_verify** | Llamarme |
| **country_code_label** | Código de país |
| **cancel_message** | El usuario ha cancelado la autenticación multifactor. |
| **text_button_send_second_code** | Envía un código nuevo |
| **code_pattern** | \\d{6} |
| **intro_mixed** | Entre sus datos nos consta el número siguiente. Podemos enviarle un código por SMS o por teléfono para autenticar su identidad. |
| **intro_mixed_p** | Entre sus datos nos constan los números siguientes. Elija un número al que podamos llamarle o enviarle un código por SMS para autenticar su identidad. |
| **button_verify_code** | Verifique el código |
| **requiredField_code** | Escriba el código de verificación que ha recibido. |
| **invalid_code** | Escriba el código de seis dígitos que ha recibido. |
| **button_cancel** | Cancelar |
| **local_number_input_placeholder_text** | Número de teléfono |
| **button_retry** | Reintento |
| **alternative_text** | No tengo mi teléfono |
| **intro_phone_p** | Entre sus datos nos constan los números siguientes. Elija un número al que podamos llamarle para autenticar su identidad. |
| **intro_phone** | Entre sus datos nos consta el número siguiente. Le llamaremos para autenticar su identidad. |
| **enter_code_text_intro** | Escriba su código de verificación a continuación.  |
| **intro_entry_phone** | Escriba un número a continuación al que podamos llamar para autenticar su identidad. |
| **intro_entry_sms** | Escriba un número a continuación al que podamos enviarle un código por SMS para autenticar su identidad. |
| **button_send_code** | Envié el código |
| **invalid_number** | Escriba un número de teléfono válido. |
| **intro_sms** | Entre sus datos nos consta el número siguiente. Le enviaremos un código por SMS para autenticar su identidad. |
| **intro_entry_mixed** | Escriba un número a continuación al que podamos llamar o enviar un código por SMS para autenticar su identidad. |
| **number_pattern** | ^\\+(?:[0-9][\\x20-]?){6,14}[0-9]$ |
| **intro_sms_p** |Entre sus datos nos constan los números siguientes. Elija un número al que podamos enviarle un código por SMS para autenticar su identidad. |
| **requiredField_countryCode** | Seleccione su código de país. |
| **requiredField_number** | Escriba su número de teléfono. |
| **country_code_input_placeholder_text** |País o región |
| **number_label** | Número de teléfono |
| **error_tryagain** | El número de teléfono que ha proporcionado está ocupado o no está disponible. Compruebe el número y vuelva a intentarlo. |
| **error_incorrect_code** | El código de verificación que ha escrito no figura en nuestros registros. Inténtelo de nuevo o solicite uno nuevo. |
| **countryList** | {\"DEFAULT\":\"Country/Region\",\"AF\":\"Afghanistan\",\"AX\":\"Åland Islands\",\"AL\":\"Albania\",\"DZ\":\"Algeria\",\"AS\":\"American Samoa\",\"AD\":\"Andorra\",\"AO\":\"Angola\",\"AI\":\"Anguilla\",\"AQ\":\"Antarctica\",\"AG\":\"Antigua and Barbuda\",\"AR\":\"Argentina\",\"AM\":\"Armenia\",\"AW\":\"Aruba\",\"AU\":\"Australia\",\"AT\":\"Austria\",\"AZ\":\"Azerbaijan\",\"BS\":\"Bahamas\",\"BH\":\"Bahrain\",\"BD\":\"Bangladesh\",\"BB\":\"Barbados\",\"BY\":\"Belarus\",\"BE\":\"Belgium\",\"BZ\":\"Belize\",\"BJ\":\"Benin\",\"BM\":\"Bermuda\",\"BT\":\"Bhutan\",\"BO\":\"Bolivia\",\"BQ\":\"Bonaire\",\"BA\":\"Bosnia and Herzegovina\",\"BW\":\"Botswana\",\"BV\":\"Bouvet Island\",\"BR\":\"Brazil\",\"IO\":\"British Indian Ocean Territory\",\"VG\":\"British Virgin Islands\",\"BN\":\"Brunei\",\"BG\":\"Bulgaria\",\"BF\":\"Burkina Faso\",\"BI\":\"Burundi\",\"CV\":\"Cabo Verde\",\"KH\":\"Cambodia\",\"CM\":\"Cameroon\",\"CA\":\"Canada\",\"KY\":\"Cayman Islands\",\"CF\":\"Central African Republic\",\"TD\":\"Chad\",\"CL\":\"Chile\",\"CN\":\"China\",\"CX\":\"Christmas Island\",\"CC\":\"Cocos (Keeling) Islands\",\"CO\":\"Colombia\",\"KM\":\"Comoros\",\"CG\":\"Congo\",\"CD\":\"Congo (DRC)\",\"CK\":\"Cook Islands\",\"CR\":\"Costa Rica\",\"CI\":\"Côte d’Ivoire\",\"HR\":\"Croatia\",\"CU\":\"Cuba\",\"CW\":\"Curaçao\",\"CY\":\"Cyprus\",\"CZ\":\"Czech Republic\",\"DK\":\"Denmark\",\"DJ\":\"Djibouti\",\"DM\":\"Dominica\",\"DO\":\"Dominican Republic\",\"EC\":\"Ecuador\",\"EG\":\"Egypt\",\"SV\":\"El Salvador\",\"GQ\":\"Equatorial Guinea\",\"ER\":\"Eritrea\",\"EE\":\"Estonia\",\"ET\":\"Ethiopia\",\"FK\":\"Falkland Islands\",\"FO\":\"Faroe Islands\",\"FJ\":\"Fiji\",\"FI\":\"Finland\",\"FR\":\"France\",\"GF\":\"French Guiana\",\"PF\":\"French Polynesia\",\"TF\":\"French Southern Territories\",\"GA\":\"Gabon\",\"GM\":\"Gambia\",\"GE\":\"Georgia\",\"DE\":\"Germany\",\"GH\":\"Ghana\",\"GI\":\"Gibraltar\",\"GR\":\"Greece\",\"GL\":\"Greenland\",\"GD\":\"Grenada\",\"GP\":\"Guadeloupe\",\"GU\":\"Guam\",\"GT\":\"Guatemala\",\"GG\":\"Guernsey\",\"GN\":\"Guinea\",\"GW\":\"Guinea-Bissau\",\"GY\":\"Guyana\",\"HT\":\"Haiti\",\"HM\":\"Heard Island and McDonald Islands\",\"HN\":\"Honduras\",\"HK\":\"Hong Kong SAR\",\"HU\":\"Hungary\",\"IS\":\"Iceland\",\"IN\":\"India\",\"ID\":\"Indonesia\",\"IR\":\"Iran\",\"IQ\":\"Iraq\",\"IE\":\"Ireland\",\"IM\":\"Isle of Man\",\"IL\":\"Israel\",\"IT\":\"Italy\",\"JM\":\"Jamaica\",\"JP\":\"Japan\",\"JE\":\"Jersey\",\"JO\":\"Jordan\",\"KZ\":\"Kazakhstan\",\"KE\":\"Kenya\",\"KI\":\"Kiribati\",\"KR\":\"Korea\",\"KW\":\"Kuwait\",\"KG\":\"Kyrgyzstan\",\"LA\":\"Laos\",\"LV\":\"Latvia\",\"LB\":\"Lebanon\",\"LS\":\"Lesotho\",\"LR\":\"Liberia\",\"LY\":\"Libya\",\"LI\":\"Liechtenstein\",\"LT\":\"Lithuania\",\"LU\":\"Luxembourg\",\"MO\":\"Macao SAR\",\"MK\":\"North Macedonia\",\"MG\":\"Madagascar\",\"MW\":\"Malawi\",\"MY\":\"Malaysia\",\"MV\":\"Maldives\",\"ML\":\"Mali\",\"MT\":\"Malta\",\"MH\":\"Marshall Islands\",\"MQ\":\"Martinique\",\"MR\":\"Mauritania\",\"MU\":\"Mauritius\",\"YT\":\"Mayotte\",\"MX\":\"Mexico\",\"FM\":\"Micronesia\",\"MD\":\"Moldova\",\"MC\":\"Monaco\",\"MN\":\"Mongolia\",\"ME\":\"Montenegro\",\"MS\":\"Montserrat\",\"MA\":\"Morocco\",\"MZ\":\"Mozambique\",\"MM\":\"Myanmar\",\"NA\":\"Namibia\",\"NR\":\"Nauru\",\"NP\":\"Nepal\",\"NL\":\"Netherlands\",\"NC\":\"New Caledonia\",\"NZ\":\"New Zealand\",\"NI\":\"Nicaragua\",\"NE\":\"Niger\",\"NG\":\"Nigeria\",\"NU\":\"Niue\",\"NF\":\"Norfolk Island\",\"KP\":\"North Korea\",\"MP\":\"Northern Mariana Islands\",\"NO\":\"Norway\",\"OM\":\"Oman\",\"PK\":\"Pakistan\",\"PW\":\"Palau\",\"PS\":\"Palestinian Authority\",\"PA\":\"Panama\",\"PG\":\"Papua New Guinea\",\"PY\":\"Paraguay\",\"PE\":\"Peru\",\"PH\":\"Philippines\",\"PN\":\"Pitcairn Islands\",\"PL\":\"Poland\",\"PT\":\"Portugal\",\"PR\":\"Puerto Rico\",\"QA\":\"Qatar\",\"RE\":\"Réunion\",\"RO\":\"Romania\",\"RU\":\"Russia\",\"RW\":\"Rwanda\",\"BL\":\"Saint Barthélemy\",\"KN\":\"Saint Kitts and Nevis\",\"LC\":\"Saint Lucia\",\"MF\":\"Saint Martin\",\"PM\":\"Saint Pierre and Miquelon\",\"VC\":\"Saint Vincent and the Grenadines\",\"WS\":\"Samoa\",\"SM\":\"San Marino\",\"ST\":\"São Tomé and Príncipe\",\"SA\":\"Saudi Arabia\",\"SN\":\"Senegal\",\"RS\":\"Serbia\",\"SC\":\"Seychelles\",\"SL\":\"Sierra Leone\",\"SG\":\"Singapore\",\"SX\":\"Sint Maarten\",\"SK\":\"Slovakia\",\"SI\":\"Slovenia\",\"SB\":\"Solomon Islands\",\"SO\":\"Somalia\",\"ZA\":\"South Africa\",\"GS\":\"South Georgia and South Sandwich Islands\",\"SS\":\"South Sudan\",\"ES\":\"Spain\",\"LK\":\"Sri Lanka\",\"SH\":\"St Helena, Ascension, Tristan da Cunha\",\"SD\":\"Sudan\",\"SR\":\"Suriname\",\"SJ\":\"Svalbard\",\"SZ\":\"Swaziland\",\"SE\":\"Sweden\",\"CH\":\"Switzerland\",\"SY\":\"Syria\",\"TW\":\"Taiwan\",\"TJ\":\"Tajikistan\",\"TZ\":\"Tanzania\",\"TH\":\"Thailand\",\"TL\":\"Timor-Leste\",\"TG\":\"Togo\",\"TK\":\"Tokelau\",\"TO\":\"Tonga\",\"TT\":\"Trinidad and Tobago\",\"TN\":\"Tunisia\",\"TR\":\"Turkey\",\"TM\":\"Turkmenistan\",\"TC\":\"Turks and Caicos Islands\",\"TV\":\"Tuvalu\",\"UM\":\"U.S. alejadas de los EE. UU.\",\"VI\":\"Islas Vírgenes de los Estados Unidos\",\"UG\":\"Uganda\",\"UA\":\"Ucrania\",\"AE\":\"Emiratos Árabes Unidos\",\"GB\":\"Reino Unido\",\"US\":\"Estados Unidos\",\"UY\":\"Uruguay\",\"UZ\":\"Uzbekistán\",\"VU\":\"Vanuatu\",\"VA\":\"Ciudad del Vaticano\",\"VE\":\"Venezuela\",\"VN\":\"Vietnam\",\"WF\":\"Wallis y Futuna\",\"YE\":\"Yemen\",\"ZM\":\"Zambia\",\"ZW\":\"Zimbabue\"} |
| **error_448** | No se puede establecer contacto con el número de teléfono que ha proporcionado. |
| **error_449** | El usuario ha superado el número de reintentos permitidos. |
| **verification_code_input_placeholder_text** | Código de verificación |

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de inscripción a la autenticación multifactor:

![Elementos de la experiencia de usuario al comprobar el correo electrónico en la página de registro](./media/localization-string-ids/localization-mfa1.png)

En el ejemplo siguiente se muestra el uso de algunos de los elementos de la interfaz de usuario en la página de validación de la autenticación multifactor:

![Elementos de la experiencia de usuario al comprobar el correo electrónico en la página de registro](./media/localization-string-ids/localization-mfa2.png)

## <a name="verification-display-control-user-interface-elements"></a>Elementos de la interfaz de usuario del control de pantalla de verificación

Los siguientes son los identificadores de un [control de pantalla de verificación](display-control-verification.md).

| id | Valor predeterminado |
| -- | ------------- |
|verification_control_but_change_claims |Change |
|verification_control_fail_send_code |No se pudo enviar el código; vuelva a intentarlo más tarde. |
|verification_control_fail_verify_code |No se pudo comprobar el código; vuelva a intentarlo más tarde. |
|verification_control_but_send_code |Envié el código |
|verification_control_but_send_new_code |Envíe un código nuevo. |
|verification_control_but_verify_code |Verifique el código |

## <a name="one-time-password-error-messages"></a>Mensajes de error de contraseña única
Los siguientes son los identificadores de los mensajes de error de un [perfil técnico de contraseña única](one-time-password-technical-profile.md).

| id | Valor predeterminado |
| -- | ------------- |
|UserMessageIfMaxRetryAttempted |La comprobación proporcionada de la contraseña única ha superado el número máximo de intentos. |
|UserMessageIfSessionDoesNotExist |La sesión de comprobación de contraseña única ha expirado. |
|UserMessageIfSessionConflict |La sesión de comprobación de contraseña única presenta un conflicto. |
|UserMessageIfInvalidCode |La contraseña única que se proporcionó para la comprobación no es correcta. |








