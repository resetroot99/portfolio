# InvOCR

AI-Powered Invoice Processing for Collision Repair Shops

## Overview

InvOCR is an autonomous agent system that ingests invoices via multiple channels (email, SMS, Telegram, Discord) and automatically extracts line items for posting to collision repair estimating platforms. The system eliminates manual data entry for estimators, reducing errors and saving hours of administrative work per day.

## Problem

Collision repair shops receive dozens of invoices daily from parts suppliers, sublet vendors, and service providers. Estimators must manually transcribe each line item from these invoices into their estimating system (typically CCC ONE). This process is time-consuming, error-prone, and prevents estimators from focusing on higher-value work like customer communication and quality control.

## Solution

InvOCR provides a multi-channel intake system where invoices can be submitted via email, SMS, or chat applications. The AI agent extracts invoice data using OCR and structured data extraction, validates line items against parts catalogs and labor guides, maps invoice data to estimating system formats (CIECA standard), and prepares data for automatic posting to CCC ONE or other platforms.

## Technical Architecture

The system implements multi-channel ingestion through email (IMAP/SMTP), SMS (Twilio), and chat platforms (Telegram, Discord). Document processing uses OCR for scanned invoices, PDF text extraction for digital invoices, and image preprocessing for quality enhancement. AI-powered extraction leverages OpenAI for structured data extraction, custom models for invoice layout understanding, and validation against known vendor formats. The integration layer handles CCC ONE API integration, CIECA XML generation, and error handling with manual review queues.

## Real-World Impact

InvOCR was successfully deployed in collision repair shops and demonstrated significant value. However, CCC ONE (the dominant estimating platform) blocked third-party API integration to protect their market position. This experience led to the development of CrashCodex, which operates at a higher abstraction layer where CCC cannot block access.

## Key Learnings

This project demonstrated that incumbent platforms will block innovations that threaten their control. The strategic response was to move to a reasoning layer where the platform becomes a commodity. InvOCR proved the value of automation in collision repair workflows, validated the AI extraction approach for industry-specific documents, and identified the need for platform-independent intelligence layers.

## Status

Private repository. The technology and learnings from InvOCR informed the development of CrashCodex. Contact ali@jakvan.io for access.

---

**Part of the AI Systems Research & Development Portfolio**
