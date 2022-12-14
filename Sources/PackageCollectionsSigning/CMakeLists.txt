# This source file is part of the Swift open source project
#
# Copyright (c) 2021 Apple Inc. and the Swift project authors
# Licensed under Apache License v2.0 with Runtime Library Exception
#
# See http://swift.org/LICENSE.txt for license information
# See http://swift.org/CONTRIBUTORS.txt for Swift project authors

add_library(PackageCollectionsSigning STATIC
  Certificate/Certificate.swift
  Certificate/CertificatePolicy.swift
  Key/ASN1/ASN1.swift
  Key/ASN1/ASN1Error.swift
  Key/ASN1/PEMDocument.swift
  Key/ASN1/SEC1PrivateKey.swift
  Key/ASN1/SubjectPublicKeyInfo.swift
  Key/ASN1/Types/ASN1BitString.swift
  Key/ASN1/Types/ASN1Identifier.swift
  Key/ASN1/Types/ASN1Integer.swift
  Key/ASN1/Types/ASN1ObjectIdentifier.swift
  Key/ASN1/Types/ASN1OctetString.swift
  Key/BoringSSLKey.swift
  Key/Key.swift
  Key/Key+EC.swift
  Key/Key+RSA.swift
  PackageCollectionSigning.swift
  Signing/BoringSSLSigning.swift
  Signing/Signature.swift
  Signing/Signing.swift
  Signing/Signing+ECKey.swift
  Signing/Signing+RSAKey.swift
  Utilities/Base64URL.swift 
  Utilities/Utilities.swift)
target_link_libraries(PackageCollectionsSigning PUBLIC
  Basics
  PackageCollectionsModel
  TSCBasic
  $<$<NOT:$<PLATFORM_ID:Darwin>>:dispatch>
  $<$<NOT:$<PLATFORM_ID:Darwin>>:Foundation>)
target_link_libraries(PackageCollectionsSigning PRIVATE
  Crypto
  CCryptoBoringSSL
  PackageCollectionsSigningLibc)
target_include_directories(PackageCollectionsSigning PRIVATE
  $<TARGET_PROPERTY:PackageCollectionsSigningLibc,INCLUDE_DIRECTORIES>)
  
# NOTE(compnerd) workaround for CMake not setting up include flags yet
set_target_properties(PackageCollectionsSigning PROPERTIES
  INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_Swift_MODULE_DIRECTORY})
target_link_options(PackageCollectionsSigning PRIVATE
    "$<$<PLATFORM_ID:Darwin>:SHELL:-Xlinker -framework -Xlinker Security>")

set_property(GLOBAL APPEND PROPERTY SwiftPM_EXPORTS PackageCollectionsSigning)
