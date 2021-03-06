---
title: チュートリアル:Azure Active Directory と The Funding Portal の統合 | Microsoft Docs
description: Azure Active Directory と The Funding Portal の間でシングル サインオンを構成する方法について説明します。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 096f251c776f87a2ab119ce394aa4825e77ee65c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189584"
---
# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>チュートリアル:Azure Active Directory と The Funding Portal の統合

このチュートリアルでは、The Funding Portal と Azure Active Directory (Azure AD) を統合する方法について説明します。

The Funding Portal と Azure AD の統合には、次の利点があります。

- The Funding Portal にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで The Funding Portal に自動的にサインオン (シングル サインオン) する機能を有効にすることができます。
- 1 つの中央サイト (Azure Portal) でアカウントを管理できます

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

Azure AD と The Funding Portal の統合を構成するには、次のアイテムが必要です。

- Azure AD サブスクリプション
- The Funding Portal でのシングル サインオンが有効なサブスクリプション

> [!NOTE]
> このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、 [こちら](https://azure.microsoft.com/pricing/free-trial/)から 1 か月の評価版を入手できます。

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。 このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの The Funding Portal の追加
1. Azure AD シングル サインオンの構成とテスト

## <a name="adding-the-funding-portal-from-the-gallery"></a>ギャラリーからの The Funding Portal の追加
Azure AD への The Funding Portal の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に The Funding Portal を追加する必要があります。

**ギャラリーから The Funding Portal を追加するには、次の手順を実行します。**

1. **[Azure Portal](https://portal.azure.com)** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。 

    ![Active Directory][1]

1. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![[アプリケーション]][2]
    
1. 新しいアプリケーションを追加するには、ダイアログの上部にある **[新しいアプリケーション]** をクリックします。

    ![[アプリケーション]][3]

1. 検索ボックスに「 **The Funding Portal**」と入力します。

    ![Azure AD のテスト ユーザーの作成](./media/thefundingportal-tutorial/tutorial_thefundingportal_search.png)

1. 結果ウィンドウで **[The Funding Portal]** を選択し、**[追加]** をクリックしてアプリケーションを追加します。

    ![Azure AD のテスト ユーザーの作成](./media/thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト
このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、The Funding Portal で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する The Funding Portal ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと The Funding Portal の関連ユーザーの間で、リンク関係が確立されている必要があります。

The Funding Portal で、Azure AD の **[ユーザー名]** の値を **[Username]** の値として割り当ててリンク関係を確立します。

The Funding Portal で Azure AD のシングル サインオンを構成してテストするには、次の手順を完了する必要があります。

1. **[Azure AD シングル サインオンの構成](#configuring-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
1. **[Azure AD のテスト ユーザーの作成](#creating-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
1. **[The Funding Portal テスト ユーザーの作成](#creating-the-funding-portal-test-user)** - The Funding Portal で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクします。
1. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Britta Simon が Azure AD のシングル サインオンを使用できるようにします。
1. **[シングル サインオンのテスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure Portal で Azure AD のシングル サインオンを有効にして、The Funding Portal アプリケーションでシングル サインオンを構成します。

**The Funding Portal で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure Portal の **The Funding Portal** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![Configure single sign-on][4]

1. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。
 
    ![Configure single sign-on](./media/thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

1. **[The Funding Portal のドメインと URL]** セクションで、次の手順を実行します。

    ![Configure single sign-on](./media/thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    a. **[サインオン URL]** ボックスに、`https://<subdomain>.regenteducation.net/` のパターンを使用して URL を入力します。

    b. **[識別子]** ボックスに、`https://<subdomain>.regenteducation.net` の形式で URL を入力します。

    > [!NOTE] 
    > これらは実際の値ではありません。 実際のサインオン URL と識別子でこれらの値を更新してください。 これらの値を取得するには、[The Funding Portal クライアント サポート チーム](mailto:info@regenteducation.com)にお問い合わせください。 

1. The Funding Portal アプリケーションでは、SAML アサーションに、"externalId1" という名前の属性を含めます。 "externalId1" の値は、認識された StudentID です。 このアプリケーションには、"externalId1" の要求を構成してください。 これらの属性の値は、アプリケーションの **[ユーザー属性]** から管理できます。 次のスクリーンショットはその例です。

    ![Configure single sign-on](./media/thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

1. **[シングル サインオン]** ダイアログの **[ユーザー属性]** セクションで、図に示すように SAML トークン属性を構成し、次の手順を実行します。

    | 属性名 | 属性値 |
    | ------------------- | ---------------- |
    | externalId1 | user.extensionattribute1 |

    a. **[属性の追加]** をクリックして **[属性の追加]** ダイアログを開きます。

    ![Configure single sign-on](./media/thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Configure single sign-on](./media/thefundingportal-tutorial/tutorial_attribute_05.png)

    b. **[名前]** ボックスに、その行に対して表示される属性名を入力します。

    c. **[属性値]** 一覧で、実装で使用する属性を選択します。 たとえば、ExtensionAttribute1 に StudentID の値を格納した場合、user.extensionattribute1 を選択します。
    
    d.[Tableau Server return URL]: Tableau Server ユーザーがアクセスする URL。 **[OK]** をクリックします。
 
1. **[SAML 署名証明書]** セクションで、**[Metadata XML (メタデータ XML)]** をクリックし、コンピューターにメタデータ ファイルを保存します。

    ![Configure single sign-on](./media/thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

1. **[保存]** ボタンをクリックします。

    ![Configure single sign-on](./media/thefundingportal-tutorial/tutorial_general_400.png)

1. **The Funding Portal** 側にシングル サインオンを構成するには、ダウンロードした**メタデータ XML** を [The Funding Portal サポート チーム](mailto:info@regenteducation.com)に送信する必要があります。 サポート チームはこれを設定して、SAML SSO 接続が両方の側で正しく設定されるようにします。

> [!TIP]
> アプリのセットアップ中、[Azure Portal](https://portal.azure.com) 内で上記の手順の簡易版を確認できるようになりました。  **[Active Directory] の [エンタープライズ アプリケーション]** セクションからこのアプリを追加した後、**[シングル サインオン]** タブをクリックし、一番下の **[構成]** セクションから組み込みドキュメントにアクセスするだけです。 埋め込みドキュメント機能の詳細については、[Azure AD の埋め込みドキュメント]( https://go.microsoft.com/fwlink/?linkid=845985)に関するページを参照してください。
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure Portal で Britta Simon というテスト ユーザーを作成することです。

![Azure AD ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure Portal** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。

    ![Azure AD のテスト ユーザーの作成](./media/thefundingportal-tutorial/create_aaduser_01.png) 

1. **[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックして、ユーザーの一覧を表示します。
    
    ![Azure AD のテスト ユーザーの作成](./media/thefundingportal-tutorial/create_aaduser_02.png) 

1. ダイアログの上部にある **[追加]** をクリックして、**[ユーザー]** ダイアログを開きます。
 
    ![Azure AD のテスト ユーザーの作成](./media/thefundingportal-tutorial/create_aaduser_03.png) 

1. **[ユーザー]** ダイアログ ページで、次の手順を実行します。
 
    ![Azure AD のテスト ユーザーの作成](./media/thefundingportal-tutorial/create_aaduser_04.png) 

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに BrittaSimon の**電子メール アドレス**を入力します。

    c. **[パスワードを表示]** を選択し、**[パスワード]** の値をメモします。

    d.[Tableau Server return URL]: Tableau Server ユーザーがアクセスする URL。 **Create** をクリックしてください。
 
### <a name="creating-the-funding-portal-test-user"></a>The Funding Portal テスト ユーザーの作成

このセクションでは、The Funding Portal で Britta Simon というユーザーを作成します。 [The Funding Portal サポート チーム](mailto:info@regenteducation.com)と連携して、テスト ユーザーを追加し、SSO を有効にします。

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に The Funding Portal へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

![ユーザーの割り当て][200] 

**The Funding Portal に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure Portal でアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201] 

1. アプリケーションの一覧で **[The Funding Portal]** を選択します。

    ![Configure single sign-on](./media/thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

1. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![ユーザーの割り当て][202] 

1. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![ユーザーの割り当て][203]

1. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。

1. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。

1. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。
    
### <a name="testing-single-sign-on"></a>シングル サインオンのテスト

このセクションの目的は、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストすることです。

アクセス パネルで The Funding Portal のタイルをクリックすると、The Funding Portal アプリケーションに自動的にサインオンします。

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/thefundingportal-tutorial/tutorial_general_203.png

